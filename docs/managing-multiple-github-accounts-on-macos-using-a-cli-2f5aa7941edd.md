# 使用 CLI 管理 macOS 上的多个 GitHub 帐户

> 原文：<https://medium.com/swlh/managing-multiple-github-accounts-on-macos-using-a-cli-2f5aa7941edd>

![](img/1bde1702704845551edca691827644e3.png)

作为一名开发人员，您可能遇到过与使用多个 GitHub 帐户相关的问题。

有几种方法可以解决这个问题。第一种方法是手动更新 git 配置，并从 keychain 中删除任何相关的密码。这种方法并不理想，因为每次你想换账户时都必须登录。当你不得不定期地在账户间切换时，这变得特别烦人。

另一个主要方法是使用 SSH。这种方法要好得多，因为您只需设置 SSH 配置，然后在 git 命令中使用正确的 URL 格式，就万事大吉了。

我打算把这个方法更进一步，让使用多个 GitHub 账户成为一个梦想。为此，我将使用一个 SSH 配置和一个用 NodeJS 编写的自定义命令行界面。

# 设置 SSH

我们需要做的第一件事是创建几个 SSH 密钥。我们需要每个用户一个。对于本文，假设我们有两个帐户。

我们将打开终端(或您使用的任何命令行应用程序)并将目录更改为我们的。ssh 文件夹。的。ssh 文件夹应该在您的根用户目录中。如果您还没有. ssh 文件夹，那么您可以通过执行以下操作来创建它:

```
cd ~
mkdir .ssh
cd .ssh
```

现在我们需要创建我们的密钥，为您想要管理的每个用户运行下面的命令一次。

```
ssh-keygen -t rsa -b 4096 -C "[your@email.com](mailto:your@email.com)"
```

我们在这里做的是使用`ssh-keygen`命令生成 rsa 类型的公钥和私钥，大小为 4096 位，然后向其中添加包含我们的电子邮件的注释。

当您运行`ssh-keygen`命令时，系统会提示您输入用于存储密钥的文件名。这个文件名需要是唯一的，在这个例子中使用您的 GitHub 用户名作为文件名。

系统还会提示您输入密码。对于这个示例，我将把它留空。如果你想使用密码，并想知道更多，那么 GitHub 有一些关于这方面的文档。

[https://help . github . com/en/articles/working-with-ssh-key-pass phrases](https://help.github.com/en/articles/working-with-ssh-key-passphrases)

一旦你的密钥已经生成，运行`ls`命令，这将允许你看到所有已经创建的密钥。对于每个用户，你应该有一个不带扩展名的，这是私钥，一个带. pub 扩展名的，这是公钥。

现在我们有了密钥，我们可以创建配置文件了。如果您的中还没有名为`config`的文件。ssh 文件夹，然后运行以下命令来创建它:

```
touch config
```

在编辑器中打开该文件，并在其中键入以下内容:

```
# GitHub username
Host github.com-username
    HostName github.com
    User git
    IdentityFile ~/.ssh/{key}
    IdentitiesOnly yes
```

将 username 替换为您的 GitHub 用户名，将`{key}`替换为您提供的与该帐户相关的密钥的文件名。我们希望我们要管理的每个用户都有一个这样的块。

一旦你完成了，我们只需要再做一步，然后我们的 SSH 就设置好了。这一步是上传密钥到 GitHub，登录你的每个账户，进入设置，然后 SSH 和 GPG 密钥。我们想要创建一个新的 SSH 密钥，所以单击 new SSH key 按钮。现在，您可以将您的公钥文件的内容粘贴到 key 字段中，并给它一个 GitHub 特定的标题。

完成这项工作后，我们可以继续构建 CLI。

# 构建 CLI

在这个示例中，我将调用 CLI git-config，但是您可以随意调用它。

在某处创建一个以您的 cli 命名的文件夹，并导航到该文件夹。我们将把所有的代码放在这里。我们将首先运行`npm init`来设置我们的 package.json 文件。浏览所有提示并输入您想要的值。一旦你完成了与`npm init`打开文件，我们需要添加一件事给它。用您的入口点的值添加键 bin，在我的例子中是`"bin": "./index.js"`。

我们还需要安装一些软件包来帮助我们的生活变得更容易。我们需要 commander、数据存储和即时异步。Commander 是一个可以让我们快速构建 cli 的包，data-store 可以让我们存储 git 配置，prompt-async 可以让我们轻松地在命令行中显示提示。运行以下命令来安装这些组件:

```
npm install commander data-store prompt-async --save
```

一旦 npm 完成了软件包的安装，您应该有一个如下所示的 package.json:

```
{
  "name": "git-cli",
  "version": "1.0.0",
  "description": "CLI to help with managing multiple GitHub accounts",
  "main": "index.js",
  "bin": "./index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Will Swan",
  "license": "UNLICENSED",
  "dependencies": {
    "commander": "^2.20.0",
    "data-store": "^3.1.0",
    "prompt-async": "^0.9.9"
  }
}
```

我们现在已经准备好开始构建 cli 了。让我们从创建入口点文件开始。

# 编写入口点

在入口点文件中键入以下代码:

```
#!/usr/bin/env node// Packages
const Program = require('commander');// Src
const Config = require('./src/Config');
const Push = require('./src/Push');
const Clone = require('./src/Clone');// CLI details
Program
  .version('1.0.0')
  .description('git-cli App');// Config command
Program
  .command('config <name>')
  .description('Switch between configurations')
  .action((name) => {
    Config.handler(name);
  }).parse(process.argv);// Push command
Program
  .command('push <version>')
  .description('Push changes to GitHub')
  .action((version) => {
    Push.handler(version);
  }).parse(process.argv)// Clone command
Program
  .command('clone <repoName>')
  .description('Clone a GitHub repo')
  .action((repoName) => {
    Clone.handler(repoName);
  }).parse(process.argv);// Parse arguments
Program.parse(process.argv);
```

现在让我们来看看这是怎么回事。

我们从以下内容开始:

```
#!/usr/bin/env node
```

这是使用节点运行 cli 所必需的。

然后，我们继续导入 commander，我们还导入了三个源文件。这些文件将包含我们每个命令的代码。

之后，我们用一个版本和描述来设置 cli。如果您运行 help 命令，那么这些应该会显示在命令行中。

接下来，我们创建三个命令:配置、推送和克隆。所有这些命令都有一个必需的参数，在 config 命令中，这是`<name>`。他们也有描述。运行 help 命令时，应该会显示说明。最后，它们都有一个动作，这是编写命令的所有逻辑的地方，在我们的例子中，我们调用源文件中的处理程序并传递参数。

然后我们有一行解析所有传入的参数。

目前，如果你试图运行它，它不会工作，你会得到一个 bash 错误，说命令没有找到。这是因为我们还没有创建任何符号链接，我们可以通过运行`npm link`来完成。运行此命令后，您应该能够像访问任何其他命令一样访问 cli。例如，如果我们运行:

```
git-cli config TestName
```

这现在可以工作了，你会得到一个错误，因为我们还没有创建我们的源文件，但是命令会被找到。

现在让我们创建我们的`src`文件夹，并添加我们的三个源文件。

```
mkdir src
touch ./src/Config.js
touch ./src/Push.js
touch ./src/Clone.js
```

# 编写配置命令

我们将一点一点地查看我们的 Config.js 文件。首先，我们需要导入一些包。

```
// Packages
const Prompt = require('prompt-async');
const Store = require('data-store');
const store = new Store({ path: '/Users/username/git-config.json'});
const util = require('util');
const exec = util.promisify(require('child_process').exec);
```

这里，我们正在导入 prompt-async 和 data-store。然后，我们通过创建一个新的 store 对象来设置我们的数据存储，并传入我们希望保存 json 文件的位置。我们将把它存储在我们的用户根目录中。这将允许我们从任何目录运行命令，并且仍然能够访问存储。

然后我们导入 util。我们希望这样，这样我们就可以使用`promisify`，因为我们希望能够使用异步 await 运行`exec`。然后，我们进入 import exec。

接下来键入以下内容:

```
// Available configs
const configs = {
  username1: {
    name: 'username1',
    email: '[emailforaccount@domain.com](mailto:emailforaccount@domain.com)'
  },
  username2: {
    name: 'username2',
    email: '[emailforaccount@domain.com](mailto:emailforaccount@domain.com)'
  }
};
```

此配置对象将保存我们所有可用的帐户，继续添加您想要管理的所有帐户。我们将在下一步中使用这个对象。

现在让我们编写我们的处理程序，键入以下内容:

```
// Handler
exports.handler = async (name) => { // Check configs
  console.log('Checking configs...');
  let config = {};
  if (name === 'username1') {
    config.name = configs.username1.name;
    config.email = configs.username1.email;
  } else if (name === 'username2') {
    config.name = configs.username2.name;
    config.email = configs.username2.email;
  } else {
    throw 'Please provide a valid config name';
  } // Ask what repo we are working on
  Prompt.start(); try { let response = await Prompt.get({
      name: 'repoName',
      description: 'What repo are you working on?',
      required: true
    }); config.repo = response.repoName; // Store config
    store.set('git', config); console.log('Configs checked'); // Set git configs
    await setGitConfigs(config); // Set remote url
    await setRemoteURL(config); } catch(err) {
    throw err;
  }}
```

我们在处理程序中做的第一件事是创建一个空的配置对象。然后，我们检查 name 参数是否对应于 configs 对象中定义的帐户之一。如果我们得到一个匹配，那么我们就把名字和电子邮件设置为空的配置对象。如果不匹配，那么我们抛出一个错误，告诉我们提供一个有效的配置名。

之后，我们使用 prompt-async 显示一个提示，询问我们当前正在进行什么样的回购。一旦为提示符输入了一个值，我们就将该值设置为我们的 config 对象。我们的配置对象现在包含了我们需要的所有东西，所以我们使用`store.set('key', 'data)`将它保存到我们的存储中。

然后我们继续调用两个函数，`setGitConfigs()`和`setRemoteURL()`。第一个函数用于设置本地和全局 git 配置，第二个函数用于设置 git 远程 URL。

让我们编写`setGitConfigs()`函数:

```
async function setGitConfigs(config) { console.log('Setting local and global git configs...'); // Set global and local git configs
  const { stdout, stderr } = await exec(`git config --global user.name ${config.name}; git config --global user.email ${config.email}; git config user.name ${config.name}; git config user.email ${config.email};`); if (stderr) {
    throw stderr;
  } else {
    console.log('Local and global git configs set');
  }}
```

这里我们使用 exec 运行设置 git 配置的命令，就像在命令行中一样。我们等待 exec 完成，然后检查 stderr 以查看是否有错误。

我们的`setRemoteURL()`功能非常相似:

```
async function setRemoteURL(config) { console.log('Setting remote url...'); // Set remote url
  const { stdout, stderr } = await exec(`git remote set-url origin [git@github.com](mailto:git@github.com)-${config.name}:${config.name}/${config.repo}.git`); if (stderr) {
    throw stderr;
  } else {
    console.log('Remote url set');
  }}
```

这就是现在的配置命令。如果您现在运行`git-cli config yourusername`，那么您应该会看到显示的日志，让我们知道发生了什么。如果您愿意，您可以检查 git-config.json 文件以确保配置被存储，您也可以运行`git config user.name`来查看 git 配置是否已经被设置。

我们的 config 命令需要注意的一点是，它只能在已经是 git 目录的目录中运行。这是因为我们正在运行 git 命令。然而，这不应该是一个问题，因为只有在 git 目录下工作时才需要运行这个命令。

# 编写推送命令

我们在 Push.js 文件中需要的包与我们在 Config.js 文件中需要的包是相同的，所以您可以复制它们。

让我们看看如何编写处理程序:

```
// Handler
exports.handler = async (version) => { console.log('Checking config...'); // Get the stored config
  const config = store.get('git'); // Display the username for this config
  console.log('');
  console.log(`Username: ${config.name}`);
  console.log(''); // Ask if this config is correct
  Prompt.start(); try { let response = await Prompt.get({
      name: 'correct',
      description: 'Is this the correct config? Y/n',
      required: true
    }); if (response.correct.toLowerCase() === 'y') { console.log('Config checked'); // Add files
      await addFiles(); // Commit
      await commitFiles(version); // Push
      await pushFiles(); } else {
      throw 'Please use git-cli config <name> to set the correct config'
    } } catch(err) {
    throw err;
  }}
```

我们在这里做的第一件事是从 git-config.json 文件中获取存储的配置。然后，我们显示已保存配置的用户名。我们这样做是因为我们想要检查以确保存储的配置是正确的。

为了执行这个检查，我们显示一个提示，询问这是否是正确的配置，如果答案是`Y`，那么我们继续执行这个命令。如果答案不是`Y`，那么我们抛出一个错误，要求我们使用 config 命令来设置正确的配置。

如果继续这个命令，我们调用三个函数，`addFiles()`、`commitFiles()`和`pushFiles()`。让我们来看看这些函数。

```
async function addFiles() { console.log('Adding files...'); const { stdout, stderr } = await exec('git add -A'); if (stderr) {
    throw stderr;
  } else {
    console.log('Files added');
  }}
```

在我们的`addFiles()`函数中，我们使用 exec 运行 git 命令来添加文件。对于这个例子，我刚刚设置了添加所有文件，如果你愿意，你可以添加一个额外的参数到 push 命令，允许你传递你想要添加的文件。然后，我们等待 exec 完成并检查是否有错误。

提交功能非常相似:

```
async function commitFiles(version) { console.log(`Comitting version ${version}...`); const { stdout, stderr } = await exec(`git commit -m "${version}"`); if (stderr) {
    throw stderr;
  } else {
    console.log(`Version ${version} committed`);
  }}
```

这里我们运行 git 命令来提交文件，并使用 version 参数作为消息。

最后是推送功能:

```
async function pushFiles() { console.log('Pushing files'); const { stdout, stderr } = await exec('git push');
  console.log('Files pushed');}
```

你会注意到这里我们不检查任何错误，这是因为无论成功与否,`git push`都会输出到 stderr。如果有错误，cli 将退出，并显示错误。

这就是我们现在需要 push 命令的所有内容。如果您在 git 目录中运行`git-cli push 1.0.0`，那么您的更改应该被推送到正确的帐户和正确的 repo。

# 编写克隆命令

我们的最后一个命令用于克隆 repos。通常，如果克隆一个回购，您可能会复制回购 URL 并运行`git clone`，如果该回购在不同的帐户中，则可能会要求您登录该帐户。该命令旨在通过允许您执行`git-cli clone repo-name`来简化过程，只要您有正确的配置集，那么这应该会将 repo 克隆到您执行该命令的目录中，而不需要复制任何内容或输入任何其他详细信息。

就像在 Push.js 中一样，我们对 Clone.js 的导入是相同的，所以你可以复制它们。

让我们编写处理程序:

```
// Handler
exports.handler = async (repoName) => { console.log('Checking config'); // Get stored config
  const config = store.get('git'); // Display username for the stored config
  console.log('');
  console.log(`Username: ${config.name}`);
  console.log(''); // Ask if this correct config
  Prompt.start(); try { let response = await Prompt.get({
      name: 'correct',
      description: 'Is this the correct config? Y/n',
      required: true
    }); if (response.correct.toLowerCase() === 'y') { console.log('Config checked');
      console.log('Cloning repo...'); const { stdout, stderr } = await exec(`git clone [git@github.com](mailto:git@github.com)-${config.name}:${config.name}/${repoName}.git`); if (stderr.includes('Cloning into')) {
        console.log('Repo cloned');
      } else {
        throw stderr;
      } } else {
      throw 'Please use git-cli config <name> to set the correct config';
    } } catch(err) {
    throw err;
  }}
```

您会注意到我们的处理程序与 Push.js 中的非常相似。这是因为我们也想检查以确保我们在克隆命令中使用了正确的配置。

处理程序的不同之处在于，我们使用 exec 运行 git 命令来克隆 repo，包括存储在我们的配置和 repoName 参数中的详细信息，而不是调用一些函数。

和`git push`一样，`git clone`也将所有内容输出到 stderr。但是，在克隆时，我们可以通过检查 stderr 是否包含`'Cloning into'`来检查是否有错误。

这就是克隆命令的全部内容。你现在应该可以运行`git-cli clone repo-name`了。如果您使用的是正确的配置，并且该存储库存在，那么您将看到克隆的存储库。

# 结论

现在，我们已经设置了 SSH 并构建了一个 cli 来帮助我们有效地管理我们的多个 GitHub 帐户。我经常需要切换 GitHub 用户和 repos，自从创建我的 cli 以来，我一直在使用它，它加快了我的开发过程。我希望你会发现它同样有用。

如果你想了解更多关于构建 cli 和它是如何工作的，那么你可以看一个我上传的 YouTube 视频，里面详细介绍了一切。

[https://www.youtube.com/watch?v=rjELrXXF_os](https://www.youtube.com/watch?v=rjELrXXF_os)

我也把 cli 放在 GitHub 上，供大家使用。我已经有了一些我想做的改进和特性。这些在我制作的时候会推送到 GitHub。你也可以随意参与回购。我很高兴看到人们给它添加了什么。

[https://github.com/willptswan/git-cli](https://github.com/willptswan/git-cli)