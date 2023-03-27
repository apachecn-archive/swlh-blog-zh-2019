# 高性能 WordPress:使用 Varnish 的反向代理缓存

> 原文：<https://medium.com/swlh/high-performance-wordpress-reverse-proxy-caching-with-varnish-f7a622042595>

![](img/a77db8b45163d16ecbe4881787c3725a.png)

Nail Polish and Varnish (Image: [Pixabay](https://www.pexels.com/photo/colorful-colourful-equipment-manicure-355206/))

在 *Allure Media* ，我帮助监管四个高流量的 WordPress 新闻网站。

保持我们服务器稳定的一项关键技术是我们的*反向代理*。我们使用 [*清漆*](https://varnish-cache.org/) ，其他人使用 *Nginx* ，还有其他选项可供选择。

反向代理缓存我们的响应。这意味着对网页的第一个请求将是…