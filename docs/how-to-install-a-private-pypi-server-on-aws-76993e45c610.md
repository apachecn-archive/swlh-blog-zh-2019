# 如何在 AWS 上安装私有 Pypi 服务器

> 原文：<https://medium.com/swlh/how-to-install-a-private-pypi-server-on-aws-76993e45c610>

![](img/74837386c79e0d5615e3fbfd05c497d9.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **动机/背景:**

什么是 pypi 服务器？那些经常使用 python 的人会知道，pypi 基本上是托管所有我们都使用和喜爱的公共 python 包的服务器——例如，像 pandas 和 requests 这样的包。然后可以使用 pip 安装这些软件包。

```
pip install pandas
```

对于那些不熟悉 python 的人来说，可以在这里看一下公共的 pypi 服务器:

[](https://pypi.org/) [## PyPI——Python 包索引

### Python 包索引(PyPI)是 Python 编程语言的软件仓库。

pypi.org](https://pypi.org/) 

## **那么，建立自己的私人 pypi 服务器有什么意义呢？**

当你从公共 pypi 服务器安装软件包时，你正在安装其他天才写的软件包。然而，你可能在你的公司写了一些专有的东西。您希望与公司的每个人共享它，但不希望其他任何人通过公共服务器安装它。在这种情况下，建立您自己的轻量级 pypi 服务器可能是一种好方法。

在我的例子中，我们公司经常使用一个内部包来抽象从 pandas 数据框到 mysql 实例的数据上传和下载。然而，这个包只是简单地保存在一个文件夹中，并复制到每个新的项目 repo 中。我们意识到，如果我们必须从文件夹中更改任何源代码，我们将不得不为所有不同的 reposs 这样做——每个 repo 都有自己的文件夹副本。很明显，将源代码转化为自己的 github 项目，并作为一个包上传到我们自己的 pypi 服务器将是一个更好的选择。然而，从建立一个端到端的文档非常少。我们不得不到处寻找不同的片段来拼凑所有的东西。

因此，在本文的剩余部分，我将向您介绍我们使用现有的 Docker 映像和 docker-compose 在 AWS 上的 EC2 实例上设置我们自己的 pypi-server 的步骤。本指南希望您对 docker 图像和 Docker 合成有一个初步的了解。如果没有，请先阅读此处的文档:

码头工人:

[](https://docs.docker.com/) [## 码头文件

### 开始使用 Docker 尝试我们新的多部分演练，包括编写您的第一个应用程序，数据存储…

docs.docker.com](https://docs.docker.com/) 

docker-撰写:

[](https://docs.docker.com/compose/) [## Docker 撰写

### Compose 是一个定义和运行多容器 Docker 应用程序的工具。要了解有关撰写的更多信息，请参考…

docs.docker.com](https://docs.docker.com/compose/) 

在本指南的下一部分，我将假设您已经对以下内容有所了解:

1.  在 AWS 上启动 EC2 实例
2.  使用 SSH 连接到您的 EC2 实例
3.  对 bash 命令的基本理解

# **设置 EC2 实例:**

我假设您已经有了本教程的 AWS 帐户。如果没有，去 https://aws.amazon.com[设置一个。](https://aws.amazon.com)

一旦你有了一个帐户，就进入 AWS 主网站，点击屏幕右上角显示“登录控制台”的橙色按钮，登录到管理控制台。

登录后，您应该会在页面顶部的“查找服务”下看到一个搜索栏。在栏中键入 EC2，然后单击自动建议选项。

接下来，您应该会看到 *EC2 仪表板。*点击“创建实例”下的“启动实例”蓝色按钮。您将通过一系列步骤来启动 EC2 实例。第一步是为您的新实例选择一个图像。我们选择了第二种选择——Amazon Linux AMI 2018 . 03 . 0(HVM ), SSD 卷类型，因为存储库应该已经包含 Docker，并且 python 应该默认安装。

(每个步骤我就不赘述了。如果您是第一次设置 EC2 实例，请阅读 AWS 的设置指南。)

完成实例设置后，返回到 *EC2 仪表板*，您应该能够看到新实例正在启动。等待几分钟，然后使用 SSH 进入您使用以下示例创建的新实例:

```
ssh -i~/.ssh/YourPrivateKey.pem ec2-user@YourInstanceIPv4Address
```

(注意这里 ssh 命令中的用户名是 ec2-user，如果您选择 ubuntu AMI，它将改为 ubuntu@IPv4Address)

# **安装服务器软件包和依赖项:**

假设您已经使用 SSH 成功连接到您的实例，您应该会看到类似下图的内容:

![](img/439cf6de2bf69b9b6743e7c40184fc4c.png)

按照服务器提示进行软件包更新。(如果您为 AMI 使用了另一个 linux 发行版，那么您将需要使用它的本地包管理器)

```
sudo yum update
```

查看 python3 是否已经预装:

```
sudo yum list | grep python3
```

如果没有，请安装您想要使用的版本:

```
sudo yum install python36
```

接下来，安装 docker:

```
sudo yum install docker
# start the service
sudo service docker start# give docker permission to run without using sudo every time
sudo usermod -a -G docker ec2-user # different username if you are not using the amazon linux AMI# exit the instance to make sure the changes take effect
exit
```

SSH 回到实例并测试更改是否生效:

```
# check if you can run docker without the sudo command
docker info# if not, debug the previous steps. If so, run a test image
docker run hello-world# you should see a hello message from docker after running the last command
```

现在，我们需要安装 docker-compose:

```
# run each of the following commands 1 at a timesudo curl -L [https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname](https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname) -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose > /dev/null# above version might be an older version, so if you want to download the most recent version, check their releases first# give the proper permission for docker-compose
sudo chmod +x /usr/local/bin/docker-compose# create a symbolic link so you can run docker-compose by just typing docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose# check if the install worked
docker-compose --version# you should see docker-compose version x.xx.x, build xxxxxxx
```

恭喜你。服务器已经基本设置好了。现在，您只需要在 EC2 实例上设置实际的 pypi 服务器。

# **Pypi-服务器安装:**

对于实际的 pypi 服务器本身，我们将使用官方支持的 pypi 服务器 docker 映像—[pypi server/pypi server:latest](https://hub.docker.com/r/pypiserver/pypiserver)

但是在我们下载图像之前，我们需要设置一些东西。

首先，我们需要建立一个目录来存储用户名和密码，pypi 服务器将使用它们来验证上传或下载请求。我们为此使用了“htapasswd”包。

```
# install httpd-tools with yum
sudo yum install httpd-tools# switch to the user's home directory
cd# make a new directory called auth
mkdir auth# cd into the auth directory
cd auth# create a new .htpasswd file
htpasswd -sc .htpasswd <some_username># it will prompt you to enter a new password. Follow the prompts
```

不错！您刚刚添加了第一个可以使用您的私有 pypi 服务器的用户。您可以添加任意数量的新用户。但是，您需要稍微修改命令，否则您将删除旧的。htpasswd 文件。

```
# -s instead of -sc from before
htpasswd -s .htpasswd <SomeNewUsername># follow the prompts
```

现在，我们终于可以提取 pypi-server 映像并启动容器了！

```
# go to the user's home directory
cd# create a new docker-compose.yml file
touch docker-compose.yml# edit the file using vim (vim has a bit of a learning curve)
# but for now you just need to know a few things:# i means insert, you cannot type text until you press i first
# pressing esc will take you to visual mode, you cannot edit text
# in visual mode, but you can move your cursor around and use
# vim commands# vim commands start with :
# :w + [enter] will save the file
# :q + [enter] after saving will exit the file# you can combine them like this
# :wq + [enter]# let's write our docker-compose file
vim docker-compose.yml# inside vim editor...# press i to go into insert modeversion: '3.3'services:
  pypi-server:
    image: pypiserver/pypiserver:latest
    ports:
      - "8081:8080"
    container_name: pypi-server
    volumes:
      - type: bind
        source: /home/ec2-user/auth
        target: /data/auth
      - type: volume
        source: pypi-server
        target: /data/packages
    command: -P /data/auth/.htpasswd -a update,download,list /data/packages
    restart: always
volumes:
  pypi-server:# press [esc] then :wq + [enter] to save and exit the file# now print out the file you just edited to make sure it was saved
cat docker-compose.yml# you should see everything you wrote in the file
```

我们在这里做了什么？我不会详细介绍我们写的每一行，但有几个要点。

首先，端口映射是 import，因为我们不能直接向 docker 容器发出请求。相反，我们将向现有的主机 EC2 实例发出请求，因此我们将主机 8081 端口映射到 docker 容器 8080 端口。

接下来，我们通过“container_name”给容器取了一个容易记住的名字。这样，通过使用 *pypi-server* 而不是 docker 分配容器的长 id，我们将来可以很容易地找到并使用这个容器。

我们还映射了我们创建的主机目录，它包含我们的。htpasswd 文件复制到位于/data/auth 的容器卷。这允许 docker 容器中的 pypi 服务器使用我们创建的主机文件来处理身份验证，以验证传入的凭证。

然后，我们创建了一个命名卷“pypi-server ”,以映射到 docker 容器中的/data/packages 卷。这允许我们在 docker 容器中上传到 pypi 服务器的包持久保存在我们在主机上创建的命名 docker 卷中。您可以通过键入以下命令来检查该卷:

```
docker volume ls
```

否则，如果容器由于某种原因关闭，已经上传的包将会丢失。

最后，我们将*重启*字段指定为“总是”。这确保了如果容器意外关闭，它将总是重新启动。

现在，您可以使用以下命令启动容器:

```
# use docker-compose to run the container
docker-compose up -d# the -d option runs the container in the background# wait a few seconds then run
docker container ps# you should see a container running called pypi-server# exit the ec2 isntance
exit
```

恭喜你。虽然有很多步骤，但是您的私有 pypi 服务器现在已经启动并运行在云上了！**如果你已经知道如何从一个定制的 pypi 服务器上传和安装包，你可以就此打住。如果没有，继续读下去。**

# **上传软件包到您的 Pypi 服务器:**

我不会详细介绍如何将包上传到 pypi 服务器，因为已经有很多精彩的指南了，所以我将链接到一个。即使您对建立自己的 pypi 服务器不感兴趣，您也应该学习如何做到这一点，因为上传到公共服务器的过程几乎是一样的。以下是我们用来准备上传包的指南:

[](/@joel.barmettler/how-to-upload-your-python-package-to-pypi-65edc5fe9c56) [## 如何将 python 包上传到 PyPi

### PyPi 包索引是使 python 如此强大的属性之一:只需一个简单的命令，您就可以…

medium.com](/@joel.barmettler/how-to-upload-your-python-package-to-pypi-65edc5fe9c56) 

现在假设您已经阅读了上面链接的文章，您可以使用下面的命令将您的包上传到本指南前面创建的 pypi 服务器。

```
twine upload --repository-url [http://(](http://(aws)ec2 IPv4 IP address):8081 dist/*
```

如果您已经正确设置了 pypi 服务器，并为 EC2 实例输入了正确的 IP 地址，您应该会看到上传成功。请记住，如果您停止并重新启动 EC2 服务器，您的 IPv4 IP 地址将会改变，除非您从 AWS 购买静态 IP 地址。

为了验证您的包是否已经上传，您可以使用您的浏览器在 [http://(](http://(your) ec2 IPv4 IP 地址):8081 访问 pypi 服务器，您应该会看到类似如下的内容:

![](img/d4c73e8aa5aec1a9dd099b9cde267975.png)

该页面将提供一个链接，供您查看 pypi 服务器上所有可用的包。如果您刚刚上传的包出现了，那么上传肯定是成功的。

# **从您的 Pypi 服务器下载:**

现在，您可以通过运行以下命令，使用 pip 从您的 pypi 服务器安装软件包:

```
pip install --extra-index-url [http://(EC2](http://(EC2) IPv4 IP Address):8081 YourPackageName --trusted-host (EC2 IPv4 IP Address)
```

如果您不想在 pip 命令中添加额外的 url 和可信主机，您可以在 HOME 目录中设置一个. pip 目录，文件名为 pip.conf，如下所示:

```
[global]
extra-index-url = [http://(EC2](http://(EC2) IPv4 IP Address):8081/
trusted-host = (EC2 IPv4 IP Address)
```

确保您的 pip 是最新版本，尤其是在虚拟环境中。当我们的 pip 低于 19.0.0 版本时，使用上面的方法安装我们自己的包时，我们遇到了一些问题。

**你也应该考虑为你的服务器设置使用 SSL 证书的 https。**否则，http 将以明文形式通过网络流量发送您的用户名和密码。如果你现在只是想测试和学习，这很好，但是如果你真的有一些专有的东西，那么确保你尽快设置它。

对大多数人来说，读到这里就足够了。然而，我们公司还有一个用例需要花一些时间来解决。**如果你想混合使用私有包和公共包对图片进行分类，请继续阅读。**

# **如何使用自己的 Pypi 服务器构建 Docker 映像:**

当您将从 pypi 服务器下载的*命令复制到 docker 文件中时，问题就出现了。这是因为 download 命令会触发来自服务器的身份验证，这将要求您输入用户名和密码。当您从 bash 控制台使用 pip 下载包时，这是没问题的。但是，当使用 docker-compose build 构建映像时，该过程会失败，因为您无法在构建过程中手动输入凭据。不幸的是，docker-compose 构建没有交互模式。可能有其他方法绕过这一点，但这里是我们如何做到这一点。*

您需要以 ARGs 的形式在 Dockerfile 文件中添加两个变量。下面是我们使用的 docker 文件，但是去掉了敏感信息。

```
FROM python:3.7-slim-stretch
MAINTAINER pmdbt "jerry@lofty.ai" # install dependenciesRUN apt-get update && \
apt-get install -yq gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 \
libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 \
libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 \
libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 \
ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget # start building from the home path of the image
RUN cd
COPY . /app
WORKDIR /app ARG USERNAME
ARG PASSWORD RUN pip install --upgrade setuptools**# Append the username and password ARGS in front of server URL**
RUN pip install --extra-index-url [http://$USERNAME:$PASSWORD@(EC2 IPv4 IP Address):8081](http://$USERNAME:$PASSWORD@18.207.243.15:8081) LoftyDataFetcher==0.1.3 --trusted-host (EC2 IPv4 IP Address)RUN pip install -r requirements.txt 
ENV AWS_CONFIG_FILE=/root/.aws/config 
CMD ["python3", "-u","main.py"]
```

请注意，我们对最初的下载命令所做的更改只是在服务器 URL 前面添加了$USERNAME:$PASSWORD@。这允许将用户名和密码的值传递给 bash 控制台中的映像构建过程，而不是将您的实际用户名和密码暴露给公司的其他人或互联网上的其他人。

然后编写您的标准 docker-compose.yml 文件，并使用以下命令运行“build ”:

```
# pass in the username and password for your pypi server below
docker build --build-arg USERNAME=<YourUserName> --build-arg PASSWORD=<YourPassword># press enter and you should see your image starting to build and now docker will know how to handle the authentication portion of installing a package from your private pypi server
```

# **结论:**

搞定了。如果您一直遵循本指南，那么您现在应该知道如何在 AWS EC2 实例上部署您自己的带身份验证的私有 pypi 服务器，构建您自己的 python 包并上传到您的 pypi 服务器，从您的 pypi 服务器安装包，以及如何在 docker 映像构建过程中安装您自己的包。

如果您使用另一个云提供商，如 GCP 或 Azure，您应该能够忽略 AWS 设置部分，但仍然遵循指南的其余部分。

这个指南最终比我想要的要长，但是希望它能帮你省去很多我们在几个工作日里遇到的麻烦。如果任何人知道我所谈论的任何事情的更有效的过程，请随时在下面留下评论来帮助其他人。

如果您有任何问题，请随时联系我。

[***Linkedin***](https://www.linkedin.com/in/jerry-chu/)***和***[***Github***](https://github.com/pmdbt)