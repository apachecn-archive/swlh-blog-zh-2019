# Php7.3 的弃用特性！

> 原文：<https://medium.com/swlh/deprecated-features-of-php7-3-82f418a0e4ff>

## 让我们探索 Php 最新版本中所有被否决的特性…

![](img/6f824abc5cccef2f98f2b19e4c5acf9f.png)

Designing Creditz: Sai Vardhan.

# 不区分大小写的常数

不区分大小写的常量声明已被否决。将 **True** 作为第三个参数传递给 define()现在会生成一个弃用警告。不推荐使用大小写与声明不同的不区分大小写的常量。

# 命名空间断言()

不推荐在名称空间中声明一个名为 *assert()* 的函数。引擎会对 assert()函数进行特殊处理，这可能会导致在定义同名的命名空间函数时出现不一致的行为。

# 搜索非字符串针的字符串

不赞成将非字符串指针传递给字符串搜索函数。将来，针将被解释为字符串，而不是 ASCII 码点。根据预期的行为，您应该显式地将指针转换为字符串，或者执行对 [chr()](https://www.php.net/manual/en/function.chr.php) 的显式调用。以下功能会受到影响:

*   [strpos()](https://www.php.net/manual/en/function.strpos.php)
*   [strrpos()](https://www.php.net/manual/en/function.strrpos.php)
*   [stripos()](https://www.php.net/manual/en/function.stripos.php)
*   [strripos()](https://www.php.net/manual/en/function.strripos.php)
*   [strstr()](https://www.php.net/manual/en/function.strstr.php)
*   [strchr()](https://www.php.net/manual/en/function.strchr.php)
*   [strrchr()](https://www.php.net/manual/en/function.strrchr.php)
*   [stristr()](https://www.php.net/manual/en/function.stristr.php)

# 条带标签流

fgetss()函数和 string.strip_tags 流过滤器已被弃用。这也会影响到 SplFileObject::fgetss()方法和 [gzgetss()](https://www.php.net/manual/en/function.gzgetss.php) 函数。

# 数据过滤

常量`**FILTER_FLAG_SCHEME_REQUIRED**`和`**FILTER_FLAG_HOST_REQUIRED**`的显式用法现在已被否决；无论如何，这两者都暗示了`**FILTER_VALIDATE_URL**`。

# 图像处理和 GD

image2wbmp()已被弃用。

# 国际化功能

如果 PHP 与 ICU ≥ 56 链接，使用`**Normalizer::NONE**`表单会抛出一个不赞成警告。

# 多字节字符串

以下未记录的 *mbereg_*()* 别名已被弃用。使用相应的 *mb_ereg_*()* 变体代替。

*   mbregex_encoding()
*   姆贝格()
*   姆布雷吉()
*   mbereg_replace()
*   mberegi _ replace()
*   mbsplit()
*   mbereg_match()
*   mbereg_search()
*   mbereg_search_pos()
*   mbereg_search_regs()
*   mbereg_search_init()
*   mbereg_search_getregs()
*   mbereg_search_getpos()
*   mbereg_search_setpos()

# ODBC 和 DB2 函数(PDO_ODBC)

pdo_odbc.db2_instance_name ini 设置已被正式否决。从 PHP 5.1.1 开始，文档中不推荐使用它

## 资源: [Php 文档](https://www.php.net/manual/en/migration73.deprecated.php#migration73.deprecated.image)

# 结论

这篇文章将解释 Php 最新版本中各种被否决的特性。即使 Php 开发人员删除了一些特性，他们也会喜欢这种编程语言。

**如果你喜欢这篇文章，请点击拍手，给我留下你的宝贵反馈，并与你的朋友分享。**

忙碌的人们，你们好，我希望你们在阅读这篇文章时感到愉快，并且我希望你们在这里学到了很多！这是我尝试分享我所学到的东西。

**希望你在这里看到了对你有用的东西。玩得开心！不断学习新东西，下次见！😉🤓**

查看我的[推特](https://twitter.com/Sri_Programmer)、 [Github](https://github.com/srimani-programmer) 和[脸书](https://www.facebook.com/srimani.programmer)。🙂

[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)

## 这篇文章发表在 [The Startup](https://medium.com/swlh) 上，这是 Medium 最大的创业刊物，拥有+446，678 名读者。

## 订阅接收[我们的头条新闻在这里](https://growthsupply.com/the-startup-newsletter/)。

[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)