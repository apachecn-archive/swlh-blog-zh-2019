# Javascript 贝克的一打

> 原文：<https://medium.com/swlh/javascript-bakers-dozen-ce2fc192f9b5>

![](img/f1f007ee063c1e37d993cc7325b09709.png)

Photo by [Florencia Viadana](https://unsplash.com/@florenciaviadana?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/bread?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在编写 Javascript 代码的几十年中，随着它从一种前端脚本语言发展成为一个真正的发电站，我学到了一些很棒的一行程序。不，不是那些一行程序，而是一段非常简洁的代码，你会惊讶于它们的强大。但是随着强大的能力而来的是巨大的责任——谨慎地使用它，以避免将代码混淆成不可读的意大利面条。

以下是我最喜欢的 13 个 Javascript 一行程序，排名不分先后。

**所有使用 Node.js v11.x 的示例，您的用法可能因客户端浏览器而异。*

## 1.转换为布尔值

要将变量转换成一个*布尔*值而不改变原始值:

```
const myBoolean = !!myVariable;
```

双 NOTs(！！)来保持布尔值的意图并防止它翻转真值。 ***myVariable*** 不会改变，但是转换会作为一个布尔型赋值给 ***myBoolean*** 。

## 2.重复数据删除

要从数组中删除重复项，请执行以下操作:

```
const deDupe = [...new Set(myArray)];
```

要求集合中的每个值都是唯一的，将其与 spread 运算符结合将从 ***myArray*** 中产生一个重复数据删除数组。

## 3.条件属性

要在对象上有条件地设置属性，请在对象上使用 spread 运算符:

```
const myObject = { ...myProperty && { propName: myValue } };
```

如果分摊结果为空，则 ***& &*** 失败，不设置新属性；否则，如果 ***& &*** 不为空，将覆盖并设置该属性。

使用布尔表达式有条件地设置对象属性的另一种方法是:

```
const myObj = { propOne: "a", ...(booleanExpr && { propTwo: "b" })}
```

## 4.合并对象

要合并对象:

```
const mergedObject = { ...objectOne, ...objectTwo };
```

支持无限制的合并，但是如果对象之间有共享属性，具有重复属性名称的最右边的对象将覆盖另一个对象的属性。**注意这仅用于浅层合并。*

## 5.交换变量

要在不使用中介的情况下交换两个变量的值:

```
[varA, varB] = [varB, varA];
```

***varA*** 的值现在是 ***varB*** 的值，反之亦然。析构允许这在内部机制中发生。

## 6.移除虚假值

要从数组中删除所有 falsy 值，请执行以下操作:

```
const clean = dirty.filter(Boolean);
```

这将删除等同于布尔假的任何内容，包括:*空、未定义、假、零(0)和空字符串*。

## 7.转换元素类型

要将*数字*元素转换为*字符串*元素:

```
const stringArray = numberArray.map(String);
```

如果数组包含一个字符串，它将保持为一个字符串。
这也可用于将*字符串*元素转换为*数字*类型:

```
const numberArray = stringArray.map(Number);
```

## 8.析构属性别名

将属性值从一个对象重新指定给另一个对象:

```
const { original: newName } = myObject;
```

这将初始化一个名为 ***新名称*** 的新变量，并将其赋以来自***my object . original .***的值

## 9.格式化 JSON 代码

以人类可读的格式显示 JSON 代码:

```
const formatted = JSON.stringify(myObj, null, 2);
```

*stringify* 命令有三个参数。第一个是 Javascript 对象。第二个是可选函数，可以在 JSON 被字符串化时对其进行操作。最后一个参数指示要添加多少空格作为缩进来格式化 JSON。省略最后一个参数，JSON 将作为一个长行返回。如果 ***myObj*** 中有循环引用，此操作将失败。

## 10.快速数字阵列

要创建一个数组并用数字填充，索引为零:

```
const numArray = Array.from(new Array(52), (x, i) => i);
```

数组大小可以是文字或变量。这是一个为一副牌创建 52 个元素的例子。

## 11.生成 2FA 代码

要生成 2FA 的六位数代码或其他验证码:

```
const code = Math.floor(Math.random() * 1000000).toString().padStart(6, "0");
```

用***pad start****target length*参数匹配随机范围内的零的数量。

## 12.洗牌阵列

在不知道值的上下文的情况下打乱数组:

```
myArray.sort(() => { return Math.random() - 0.5});
```

一个更好的排序算法是我在我的 [Javascript Shuffle](/swlh/the-javascript-shuffle-62660df19a5d) 文章中提到的 Fisher-Yates shuffle。

## 13.深层克隆

这种方法的性能不是很好，但是如果您需要一个快速的一行程序，那么这种克隆方法可以用来深度复制一个对象:

```
const myClone = JSON.parse(JSON.stringify(originalObject));
```

如果***original object***有循环引用，那么这个会失败。在您自己创建的简单对象上使用这种技术。

使用扩展运算符可以创建浅层克隆:

```
const myClone = { ...orignalObject };
```

## 组合和扩展

有许多方法可以将这些一行程序结合起来，用很少的代码完成神奇的壮举。Javascript 将继续发展，拥有额外的超级能力，允许开发人员减少他们代码库的大小。使用这些一行程序来扩展您的技能并更快地编写代码。