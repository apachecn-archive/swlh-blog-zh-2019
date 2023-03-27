# 简化的 Java 8 函数式编程

> 原文：<https://medium.com/swlh/java-8-functional-programming-simplified-bb24707e1c00>

本文将解释 Java 8 函数式编程的相关概念，即 Lambda 表达式、函数接口等。

让我们从函数开始:函数接受输入，生成输出。

```
int sum(int num1, int num2) {
 return num1 + num2;
}
```

Java 从一开始就支持函数。一个纯函数不会改变状态，并且总是为相同的输入产生相同的输出。

在函数式编程中，函数可以作为参数传递给其他函数，并且可以…