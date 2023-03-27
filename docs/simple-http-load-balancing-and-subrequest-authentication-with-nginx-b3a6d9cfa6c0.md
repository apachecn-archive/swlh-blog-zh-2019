# 使用 NGINX 实现简单的 HTTP 负载平衡和子请求认证

> 原文：<https://medium.com/swlh/simple-http-load-balancing-and-subrequest-authentication-with-nginx-b3a6d9cfa6c0>

![](img/506a89aaddb7fddeb7f2534f631648f2.png)

Source: [www.nginx.com](https://www.nginx.com/)

NGINX 是一个高性能的 web 服务器。我们将看看如何使用它作为负载平衡器。我们还将看到如何基于子请求结果实现身份验证。您可以编写任意多的配置文件。所有配置文件都可以在以下目录中找到:

```
/etc/nginx/
```