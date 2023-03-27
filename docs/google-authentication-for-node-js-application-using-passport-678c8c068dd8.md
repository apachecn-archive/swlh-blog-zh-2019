# 使用 Passport 对节点 js 应用程序进行 Google 身份验证

> 原文：<https://medium.com/swlh/google-authentication-for-node-js-application-using-passport-678c8c068dd8>

![](img/16b09345c143d852c0b52d8c4c90a47e.png)

本文将带您了解如何在 Node js 应用程序中实现 Google 认证。为此，您必须遵循下面提到的步骤。

1.  使用 npm init 创建一个节点 js 项目。
2.  使用 npm 安装 passport 和 passport-google-oauth20。
3.  使用您的 Google 帐户登录 Google 云平台控制台。
4.  在 Google 云平台控制台中创建一个新项目并设置它
5.  在 Node js 项目中通过 Passport 实现 Google 策略。
6.  测试 Google 身份验证是否适用于您的应用程序。

我们开始吧

1.  使用 npm init 创建一个节点 js 项目。

最初，您必须创建一个文件夹，然后用 Visual Studio 代码打开它(作者建议的文本编辑器)

之后，您必须使用 npm 命令启动一个新项目，如下所示。

```
npm init
```

将包名填写为“test google ”,并一直按 enter 键，直到出现问题“这可以吗?”?(是)”。在那里键入 yes，然后按 enter 键。当您这样做时，将在您的文件夹中创建一个 package.json 文件(寻找它)

**2。使用 npm 安装 passport 和 passport-google-oauth20。**

为了实现谷歌认证，我们必须创建一个谷歌认证策略。为此，您必须使用两个 npm 模块，即 passport 和 passport-google-oauth20。要安装它们，请转到您的控制台(在 vs 代码中为 Ctrl +`键),并在您的控制台上键入下面一行。此外，安装 express，我们稍后将使用它来构建我们的 API。

```
npm i passport passport-google-oauth20
npm i express
```

安装完成后，转到 package.json 文件，检查 passport 和 passport-google-oauth20 是否列在 dependencies 下。

**3。使用您的 Google 帐户登录 Google 云平台控制台。**

要开始下一部分，请前往[https://console.cloud.google.com/](https://console.cloud.google.com/)并登录您的谷歌云控制台。

**4。在谷歌云平台控制台中创建一个新项目并设置它**

点击谷歌云平台标志旁边的**选择项目**。当您单击时，将出现一个名为“选择项目”的弹出窗口。点击弹出窗口右上方的**新建项目**。

填写新的项目表单(将项目名称填写为“test google”)，然后按下 create 按钮。

创建项目后，您必须返回到**主页**并点击**选择项目** t，此时您点击项目名称，您将进入项目的仪表板。

在 API 下的仪表板中，选择 **API 概述**进入 API 和服务部分。现在点击侧面导航栏中的**凭证**。点击**下的 OAuth 同意屏幕**并填写显示的表格。您必须填写的强制属性是应用程序名称(如“test google”)和授权域(如[YOUR_PROJECT ID].appspot.com)。填写完毕后，点击保存按钮。

完成上述操作后，单击“创建凭据”= >“OAuth 客户端 Id”。在下一部分中，单击“web application”，填写名称(如“test google”)并授权重定向为[http://localhost:3000/callback(](http://localhost:3000/callback)如果 google 身份验证成功，这将触发回调)，然后单击 create。点击后，将出现一个弹出窗口，显示 Web 客户端 id 和客户端密码，复制它们并在您的文件夹中创建 config.js 文件，并将复制的信息放在下面的格式中。

```
module.exports={
         clientId: “[your web client Id]”,
         secret: “[your client secret]”
         callback: ‘http://localhost:3000/callback'
}
```

**5。在你创建的 Node js 项目中通过 Passport 实现 Google 战略**

在文件夹中创建一个名为 googleStrategy.js 的文件。创建该文件是为了实现 google 身份验证的策略和配置。将下面的代码部分复制并粘贴到 googleStrategy.js 文件中，然后保存

```
const passport = require(‘passport’);
const GoogleStrategy = require(‘passport-google-oauth20’).Strategy;
const config = require(‘./config’)function extractProfile(profile) {
let imageUrl = ‘’;if (profile.photos && profile.photos.length) {
imageUrl = profile.photos[0].value;
}return {id: profile.id,
displayName: profile.displayName,
image: imageUrl,
};
}passport.use(new GoogleStrategy({clientID: config.clientId,
clientSecret: config.secret,
callbackURL: config.callback,
accessType: ‘offline’,
userProfileURL: ‘https://www.googleapis.com/oauth2/v3/userinfo',
},
(accessToken, refreshToken, profile, cb) => {
       cb(null, extractProfile(profile));
}));passport.serializeUser((user, cb) => {
          cb(null, user);
});passport.deserializeUser((obj, cb) => {
          cb(null, obj);
});
```

上一节中的代码用它的配置创建了一个新的 Google 策略，让 passport 在应用程序中使用它，方法是在 passport.use()中指定它。

现在，我们必须创建 API 来开始对实现的 google 认证进行测试。创建一个名为 **server.js** 的文件，并将代码包含在下面的部分中。

```
const passport = require(‘passport’);
const express = require(‘express’)
const googleStratergy = require(‘./googleStrategy’)const app = express();// app.use(googleStratergy);
app.use(passport.initialize())// Api call for google authenticationapp.get(
    ‘/’,
    passport.authenticate(‘google’, {scope:[‘email’, ‘profile’]}),            (req,res)=>{
});// Api call back function
app.get(‘/callback’
          ,passport.authenticate(‘google’, {scope: [‘email’, ‘profile’]}),
       (req,res)=>{
             return res.send(“Congrats”);
});app.listen(3000,() => console.log(‘app listening on port 3000!’));
```

在上面的代码部分中，两个 API 调用是使用 express 实现的

**6。测试 Google 身份验证是否适用于您的应用程序。**

完成上述所有步骤后，您必须测试它是否工作，为此，我们已经运行了 **server.js** 文件，因为它由 API 组成。

如果您在 Visual Studio 代码中，请按 Ctrl +`打开终端并键入

```
node server.js
```

“app 监听端口 3000！”以确认您的服务器正在本地主机端口 3000 上运行。

最后，你打开浏览器，输入[**http://localhost:3000/**](http://localhost:3000/)**，然后回车。它会将你重定向到谷歌登录屏幕。输入您的凭据并登录。如果凭证有效，它将重定向到您的回电，并在屏幕上显示“祝贺”。**