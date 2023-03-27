# JSON 网页签名(JWS)和五岁的 JWS 分开了。

> 原文：<https://medium.com/swlh/json-web-signature-jws-and-jws-detached-for-a-five-year-old-88729b7b1a68>

![](img/e4219e2a4e990501ccb41bcae3f0c39e.png)

使用 JSON Web Signature (JWS)一段时间后，我得出结论，许多开发人员不理解它是什么以及它是如何工作的。

## JSON Web Signature (JWS)代表使用基于 JSON 的数据结构通过数字签名或消息认证码(MAC)保护的内容。

JWSs 有两个已定义的序列化:一个 compact(在本文中有描述)和一个 JSON。

紧凑的序列化 JWS 是一个包含三个部分(按顺序)的字符串，用一个点("."):

*   标题，
*   有效载荷，
*   签名。

*紧凑序列化 JWS 字符串的例子:* `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9**.**eyJleHAiOjIyMTgyMzkwMjIsIm5hbWUiOiJUb21hc3ogWndpZXJ6Y2hvxYQifQ.t3VhQ7QsILDuV_HNFSMI-Fb2FoT7fuzalpS5AH8A9c0` 每一部分都被 *BASE64URL* 编码。

**报头**描述了应用于有效载荷的数字签名或消息认证码(MAC)以及 JWS 的可选附加属性。

编码前的头部分是一个 JSON 结构(如下例):

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

标题**中的标题参数名(对象键)必须**唯一。有注册的头参数名:

*   “alg”(**必需的** ) —算法，标识用于保护 JWS 的加密
    算法；
    *可以设置为‘none’，表示签名必须是空字符串！*
*   “jku”(可选)JWK Set URL 是一个 URI，它引用一组 JSON 编码的公钥的资源，其中一个对应于用于对 JWS 进行数字签名的密钥；
*   “jwk”(可选)JSON Web Key 是与用于对 JWS 进行数字签名的私钥相对应的公钥。这个公钥表示为一个 JSON Web Key[JWK]；
*   “kid”(可选)密钥 id 是指示哪个密钥用于保护 JWS 的提示。它可用于通知接收方密钥已更改；
*   “x5u”(可选)X.509 URL 是一个 URI，它指向 X.509 公钥证书或证书链的资源，该证书链对应于用于对 JWS 进行数字签名的私钥；
*   “x5c”—X.509 证书链(可选)包含 x . 509 公钥证书或证书链，对应于用于对 JWS 进行数字签名的私钥；
*   “x5t”-X.509 证书 SHA-1 指纹(指纹)(可选)是 x . 509 公共证书[RFC5280]的 DER
    编码的
    base64url 编码的 SHA-1 指纹(也称为摘要),对应于用于对 JWS 进行数字签名的私钥；
*   “x5t # S256”——(可选)X.509 证书 SHA-256 指纹(指纹)是对应于用于对 JWS 进行数字签名的私钥的 X.509 公共证书的 DER 编码的 base64url 编码的 SHA-256 指纹(也称为摘要)
    ；
*   “typ”(可选)类型由 JWS 应用程序用来声明完整 JWS 的媒体类型；
*   “cty”(可选)内容类型由 JWS 应用程序
    用来声明有效负载的媒体类型。

在我们的示例头中，我们可以看到 JWS 类型是 JSON Web Token (JWT ),有效载荷由 HS256 ( [HMAC](https://en.wikipedia.org/wiki/HMAC) 与 SHA-256)加密算法保护。

**有效载荷**可以是任何内容。*可以是 JSON 但不需要。*

**签名**是根据 ASCII(BASE64URL(UTF8(JWS 保护标头))|| ' . '中为正在使用的特定算法定义的方式计算的||BASE64URL(JWS 有效负载))。

**重要提示:不要混淆 *BASE64URL* 和 *BASE64* ！ *BASE64URL* 中的**
:

*   所有尾随的“=”都被删除
*   “+”被替换为“-”
*   “/”被替换为“_”。

## JWS 分离是 JWS 的一个变种，它允许你在不修改的情况下对 HTTP 请求或响应的内容(主体)进行签名。

添加了 HTTP 头“x-jws 签名”,它包含允许检查消息在从发送者到接收者的途中是否没有被改变的数据。

JWS 分离生成算法非常简单:
a)使用 HTTP 主体作为有效载荷，使用紧凑序列化生成标准 JWS，
b)将中间部分(有效载荷)变成空字符串，
c)将最终字符串放入 HTTP 头“x-jws 签名”

*JWS 脱弦的例子:* `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9**.**.t3VhQ7QsILDuV_HNFSMI-Fb2FoT7fuzalpS5AH8A9c0`

验证 jws 分离的 HTTP 消息也很简单:
a)获取 HTTP 头“x-jws 签名”，
b)获取 BASE64URL HTTP 主体
c)将生成的字符串 b)放入有效载荷部分
d)验证 JWS

**JWS** 经常与 **JSON Web Token (JWT)** 、 **JSON Web Encryption (JWE)** 或**JSON Object Signing and Encryption(荷西)**。很多类似的捷径可能会导致混乱。