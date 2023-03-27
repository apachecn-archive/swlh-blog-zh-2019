# 使用 Spring Boot 流式传输数据

> 原文：<https://medium.com/swlh/streaming-data-with-spring-boot-restful-web-service-87522511c071>

流式数据是一种向 web 浏览器发送数据的全新方法，它大大加快了页面加载速度。

![](img/b6db6cc339e1e14cd6b87ce64e94b4d8.png)

Spring and Spring Boot

很多时候，我们需要允许用户在 web 应用程序中下载文件。当数据太大时，提供良好的用户体验就变得相当困难。

Spring 通过 *StreamingResponseBody* 提供对异步请求处理的支持。在…