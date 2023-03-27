# 根据 Azure Key Vault 对 Windows 虚拟机进行身份验证的三种方式。

> 原文：<https://medium.com/swlh/three-ways-of-authenticating-a-windows-virtual-machine-against-azure-key-vault-60d1ac42a199>

在这篇文章中，我分享了三种让 Windows 虚拟机访问密钥库的方法。该机器可以是 azure 虚拟机，也可以是非 azure 机器，如您的个人计算机或本地服务器。

# 使用 azure 托管身份

只有当虚拟机是 azure 虚拟机时，这种方法才有效。本质上，这种方法使用需要访问 azure 密钥库的 azure 资源的身份。这种资源的例子有 azure 虚拟机…