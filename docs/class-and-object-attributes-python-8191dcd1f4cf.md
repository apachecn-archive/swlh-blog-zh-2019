# 类和对象属性 Python

> 原文：<https://medium.com/swlh/class-and-object-attributes-python-8191dcd1f4cf>

什么是类和对象属性？有什么区别？为什么？

![](img/bb5d7dbc747c4879550aeccc509b189d.png)

在进入类和对象属性的比较和例子之前，让我们首先定义它们—

*   **类属性**是属于某个类的变量，而不是特定的对象。这个类的每个实例共享同一个变量。这些属性通常在`__init__`构造函数之外定义。
*   **实例/对象属性**是属于一个(*且仅属于一个*)对象的变量。一个类的每个实例都指向它自己的属性变量。这些属性是在`__init__`构造函数中定义的。

# 为什么呢？

为什么需要使用类属性和对象属性？

在一个只有狗的平行世界里，每只狗都有名字和年龄。狗的总数必须随时更新。所有这些都必须在一个类中定义！这可能看起来像这样:

```
class Dog:
    dogs_count = 0 def __init__(self, name, age):
        self.name = name
        self.age = age
        print("Welcome to this world {}!".format(self.name))
        Dog.dogs_count += 1 def __del__(self):
        print("Goodbye {} :(".format(self.name))
        Dog.dogs_count -= 1
```

在这个类中，我们有一个类属性`dogs_count`。这个变量记录了我们的狗狗世界中狗狗的数量。我们有两个实例属性，`name`和`age`。这些变量对每只狗都是唯一的(每个实例的属性有不同的内存位置)。每次执行`__init__`功能，`dogs_count`增加。同样地——每一次狗死了(不幸的是狗不会永远活在这个世界上)，调用`__del__`方法，`dogs_count`减少。

```
a = Dog("Max", 1)
print("Number of dogs: {}".format(Dog.dogs_count))
b = Dog("Charlie", 7)
del a
c = Dog("Spot", 4.5)
print("Number of dogs: {}".format(Dog.dogs_count))
del b
del c
print("Number of dogs: {}".format(Dog.dogs_count))***Output:*** Welcome to this world Max!
Number of dogs: 1
Welcome to this world Charlie!
Goodbye Max :(
Welcome to this world Spot!
Number of dogs: 2
Goodbye Charlie :(
Goodbye Spot :(
Number of dogs: 0
```

啊哈！我们设法为一个对象分配唯一的变量，同时拥有一个所有对象都包含的共享变量。

# 属性的继承

在打开这个话题之前，我们先来看看内置的`__dict__`属性。

```
class Example:
    classAttr = 0
    def __init__(self, instanceAttr):
        self.instanceAttr = instanceAttra = Example(1)
print(a.__dict__)
print(Example.__dict__)***Output:*** {'instanceAttr': 1}
{'__module__': '__main__', '__doc__': None, '__dict__': <attribute '__dict__' of 'Example' objects>, '__init__': <function Example.__init__ at 0x7f8af2113f28>, 'classAttr': 0, '__weakref__': <attribute '__weakref__' of 'Example' objects>}
```

正如我们所看到的，类和对象都有一个包含属性键和值的字典。类字典存储实例不包含的多个内置属性。

```
b = Example(2)
print(b.classAttr)
print(Example.classAttr)
b.classAttr = 653
print(b.classAttr)
print(Example.classAttr)***Output:*** 0
0
653
0
```

**WOAH。回到我之前写的，*一个类的每个实例共享相同的类属性*。这里发生了什么？我们改变了某个实例的 class 属性，但是共享变量实际上并没有改变。看一看这些元素的字典会有更深入的了解:**

```
b = Example(2)
print(b.__dict__)
print(Example.__dict__)
b.classAttr = 653
print(b.__dict__)
print(Example.__dict__)***Output:*** {'instanceAttr': 2}
'__module__': '__main__', '__doc__': None, '__dict__': <attribute '__dict__' of 'Example' objects>, '__init__': <function Example.__init__ at 0x7f8af2113f28>, 'classAttr': 0, '__weakref__': <attribute '__weakref__' of 'Example' objects>}
**{'instanceAttr': 2, 'classAttr': 653}**
{'__module__': '__main__', '__doc__': None, '__dict__': <attribute '__dict__' of 'Example' objects>, '__init__': <function Example.__init__ at 0x7f8af2113f28>, 'classAttr': 0, '__weakref__': <attribute '__weakref__' of 'Example' objects>}
```

仔细观察，我们注意到`classAttr`已经被添加到对象的字典中，并带有修改后的值。类的字典保持不变，这表明类属性有时可以表现为实例属性。

# 结论

综上所述，类和对象属性非常有用，但是在一起使用时会变得混乱。当每个对象需要共享一个变量(如计数器)时，类属性是有利的。当每个唯一的对象需要自己的值时，对象属性具有优势，这使它们不同于其他对象。