# C++右值，移动语义，复制省略。

> 原文：<https://medium.com/swlh/c-rvalues-move-semantics-and-copy-elision-36d492da5446>

在我写了第一篇关于 c++的文章[通过值、指针和引用传递参数](/@yang.kevvy/c-pass-by-value-pointer-reference-ddc3780d907c)之后，我收到了许多关于*右值*、*移动语义*和*复制省略*的请求。对于使用其他编程语言的人来说，这些很可能是新的术语，即使是长期的 C++程序员也可能从未听说过这些术语。本文将通过一些简单明了的例子来解释这些术语的含义。

# 右值

右值是临时值。右值之所以这样叫是因为它们通常位于赋值的右侧。它们可以被赋给其他变量，但不能被赋值。例子包括非字符串文字和函数调用。在下面的例子中，`1`和`foo()`是右值。它们用于初始化变量`a`和`b`。`a`和`b`被称为左值(“左”值)。

```
int a = 1;
int b = foo();
```

下列语句无法编译。`1`和`foo()`是临时的。不可能给它们赋值，也没有语义意义。

```
1 = c; // error
foo() = d; // error
```

如何判断一个值是否是临时的？根据一般经验，如果一个值没有标识符或名称，它就是临时的。所以如果一个值被一个变量捕获，它就不是一个右值。举个例子，

```
int bar = 1; // bar is a name, so it is an lvalue.
2; // 2 is an integer literal, it has no name (no identifier). It is
   // an rvalue.
```

您可以做的另一个测试是使用地址操作符(&)。不可能获取右值的地址。

```
int a = 1;
&a; // Works, a is a stack-allocated variable.
&1; // Error, 1 is an integer literal.
&foo(); // Error, cannot take address of the temporary result of a 
        // function call.
```

右值是 C++语言的一个重要补充，因为它们允许所谓的**移动**。

# 移动语义

假设有一个大对象，它的数据正在被转移到同类型的另一个对象。在 C++和许多其他编程语言中，这可以通过昂贵的拷贝来实现。

```
SomeBigObject obj1;
SomeBigObject obj2 = obj1; // Expensive copy.
```

C++11 引入了移动的概念，使这种操作更加有效。移动允许一个对象“接管”或“窃取”另一个对象的数据。

```
SomeBigObject obj1;
SomeBigObject obj2 = std::move(obj1); // Efficient move.
```

现在`obj2`与`obj1`相同，没有复制任何数据，导致`obj2`非常有效的初始化。问题是`obj1`现在处于无效状态，因为它的数据成员刚刚被窃取。例如:

```
struct SomeBigObject {
  // Constructor and other methods.
  ... // Copy constructor.
  SomeBigObject(const SomeBigObject& other) {
    *foo = *other.foo;
  } // Move constructor.
  SomeBigObject(SomeBigObject&& other) {
    foo = other.foo;
    other.foo = nullptr;
  } // Data members. Foo is allocated on the heap and could be large.
  Foo* foo;
}
```

当一个类型为`SomeBigObject`的对象通过移动另一个相同类型的对象进行初始化时，调用*移动构造函数*。*移动构造函数*从源对象中窃取`Foo`指针，并将源对象的`Foo`指针设置为`nullptr`。这是一个`O(1)`操作，使源对象处于无效状态，其中`foo`为`nullptr`。`other`的`foo`对象被‘偷’了。

这不同于复制同类型的另一个对象，后者调用*复制构造函数*。在*复制构造函数*中，源对象的整个`Foo`成员被复制到目标对象的`foo`成员变量中，这可能是一个非常昂贵的复制。

在某些情况下，让源对象处于无效状态是不可取的，但是如果源对象是一个临时的右值，这就不是问题。移动右值非常有用。其他一些类型，如`std::unique_ptr` s，只允许移动。

# 复制省略

假设有一个大对象从一个构造函数中返回。

```
SomeBigObject BuildBigObject(){
  SomeBigObject foo;
  // Initialize foo...
  return foo;
}SomeBigObject bar = BuildBigObject();
```

为了初始化`bar`,函数作用域的`foo`必须从函数的堆栈框架中复制到外部作用域的`bar`变量中，这是一个昂贵的复制。如果有一种方法可以让`bar`成为函数作用域的`foo`(也就是说，每当使用`foo`时，就用`bar`来代替)会怎么样？).这种优化称为复制省略，与移动类似，但不完全相同。

记住`BuildBigObject`的返回值是一个临时的右值。弹出`BuildBigObject`的栈框就会消失。编译器可以检测到`foo`是一个临时的，它不会丢弃它，而是在`BuildBigObject`的调用中让`bar`和`foo`引用同一个内存位置。这意味着当`BuildBigOjbect`初始化`foo`时，它实际上是在外部作用域`bar`上工作。这种魔力被称为命名返回值优化(NRVO ),它是复制省略的一个具体应用。更多关于复制埃利斯，RVO，和 NRVO [这里](https://en.cppreference.com/w/cpp/language/copy_elision)。

分解实际发生的事情，程序首先为`bar`分配内存，并将`bar`的指针传递给`BuildBigObject`，后者使用内存而不是`foo`。现在，每当`BuildBigObject`引用`foo`时，实际上都是在解引用`bar`。这在逻辑上等同于移动，但是编译器在幕后执行的复杂细节使它不完全相同。

对于复制省略的发生，`BuildBigObject`必须遵循一些规则，这超出了本文的范围。一个简单的没有任何分支的构建函数，就像上面显示的那样，应该可以满足复制省略的需要。