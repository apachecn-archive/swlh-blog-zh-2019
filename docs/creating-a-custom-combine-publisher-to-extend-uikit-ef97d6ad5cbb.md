# 创建自定义组合发布程序以扩展 UIKit

> 原文：<https://medium.com/swlh/creating-a-custom-combine-publisher-to-extend-uikit-ef97d6ad5cbb>

自定义组合发布者可以为您每天使用的 UIKit 元素添加缺少的功能。许多样板代码可以删除，实现可以简化。

一个简单的例子是响应`UIControl`事件。标准库附带了一些很棒的扩展，例如，轻松解码 JSON 和给关键路径赋值。不幸的是，对`UIControl`事件的响应不在这里。虽然您可能会在以后的更新中用到它，但是让我们以它为例来创建我们自己的自定义组合发布器。