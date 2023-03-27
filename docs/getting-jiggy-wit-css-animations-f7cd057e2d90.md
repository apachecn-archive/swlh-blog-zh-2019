# 获得摇摆机智的 CSS 动画

> 原文：<https://medium.com/swlh/getting-jiggy-wit-css-animations-f7cd057e2d90>

> 网站上的动画看起来很酷！它们也有助于用户体验传达信息！让我们深入了解 CSS 有趣部分的基础知识。

![](img/9c12c5808784b445b3bd4d326756f970.png)

Photo by [Kobu Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我第一次决定学习如何编码时，最让我兴奋的事情之一就是网站上的动画。我喜欢这些小小的动作给我使用不同网站的体验增添了一层乐趣。我将分享一些基础知识，让你开始做一些很酷的东西！在这篇博客中，我将涵盖 3 个基本部分:

*   伪类
*   转换
*   过渡

# 伪类

因为动画是你通常希望在用户做某事时被触发的东西，所以从伪类开始很重要。伪类是您在 CSS 选择器之后添加的特殊关键字，它定义了您所选择的元素的状态，就像如果您将鼠标放在元素上，或者如果您访问了一个链接。您键入选择器，添加一个冒号，然后是伪类。

今天要讲的三个伪类是:

*   盘旋
*   活跃的
*   集中

## :悬停

当用户将光标放在所选元素上时，hover 伪类激活。

```
button:hover {
  background-color: red;
}
```

## :活动

当用户通常通过用鼠标点击按钮来“激活”按钮时，激活的伪类被激活。一个很好的例子就是当你点击按钮时，给按钮一种向下移动的视觉效果。

```
button:active {
  background-color: black;
}
```

## :聚焦

当用户单击或使用键盘上的 tab 键选择一个元素时，就会触发 focus 伪类，您在表单上键入的部分就是一个很好的例子。这听起来像是 *active* 伪类，但是 active 只在对象被点击时起作用。一旦用户放开鼠标，该元素就不再是活动的。

```
button:active {
  box-shadow: 10px 5px 5px black;
}
```

> 现在我们知道了一些激活动画的方法，让我们来谈谈一旦它们被激活后我们能做什么。

# 改变

Transform 是一个 CSS 属性，允许您更改所选元素的大小、形状和位置。您可以使用 CSS 函数作为值的*转换*属性。接下来，我们将查看三个 CSS 函数值:

*   刻度()
*   翻译()
*   旋转()

```
button {
  transform: scale(2);
}
```

## 刻度()

缩放允许您更改元素的大小。您可以在()中输入一个数字来更改缩放量。Scale 可以取一个或两个值，只需输入一个值就可以均匀地缩放元素。通过输入两个值，第一个值将水平缩放元素，第二个值将垂直缩放元素

```
button {
  transform: scale(2, 4);
}
```

您也可以使用十进制数字来缩小元素，使其变得更小。

```
button {
  transform: scale(0.5);
}
```

## **翻译()**

translate CSS 函数值水平和/或垂直移动元素。就像 scale 函数一样，translate 可以在()中接受一个或两个值。第一个值在屏幕上水平移动元素，第二个值在屏幕上垂直上下移动元素。如果要向上或向左移动元素，可以使用负数(-2)

```
button {
  transform: translate(20px, -10px); 
//will move the button 20px to the right and 10px up in the browser window
}
```

## 旋转()

rotate() CSS 函数将选定的元素旋转括号内设置的角度。如果()内的角度数值为正，则旋转方向为顺时针，如果括号内的数值为负，则旋转方向为逆时针。

```
button {
  transform: rotate(45deg);
/*will rotate the element 45 degrees clockwise*/
}
```

> 我们知道如何让元素移动，但它们仍然不是动画。下一节将展示如何让一切看起来平滑流畅

# 过渡

转场让您可以控制动画将运行多长时间以及运行哪种类型的动画(开始时慢，结束时快)。我最喜欢的编写 CSS 过渡的方法是使用速记属性。通过转换，您可以选择属性、持续时间、时间和延迟。您可以在 CSS 中将每个属性单独设置为单独的属性，也可以使用简写。

```
transition: <property> <duration> <timing-function> <delay>;
```

以下是我们所学的全部内容的完整示例:

```
/*Syntax*/button {
  background-color: blue;
  border: none;
  border-radius: 50%;
  width: 100px;
  height: 100px;
  color: white;
  transition: transform 3s ease-in;
}button:hover{
  transform: scale(0.1) rotate(180deg);
}/*This will rotate the button by 180 degrees clockwise and shrink  when you hover over it*/
```

概括一下我们已经讲过的内容:

*   伪类——一个元素可以处于不同的状态。
*   变换(transform )-更改元素的大小、位置和旋转。
*   过渡-使您的变换清晰平滑。

感谢您阅读这篇介绍我最喜欢的前端开发部分的文章！动画不仅仅是看起来很酷，它们还可以显示错误的迹象，或者在没有使用文字的情况下完成了一些事情。我期待着深入研究 CSS 动画。如果你有任何问题让我知道！