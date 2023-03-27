# Vim 易受攻击—在 macOS 上更新 Vim

> 原文：<https://medium.com/swlh/vim-is-vulnerable-update-vim-on-macos-66402e5ab46a>

## 只需几分钟就可以将 Vim 更新到最新版本。

如果你住在航站楼，你可能会经常使用 Vim。许多人对 Vim 又爱又恨，但就我个人而言，它是我首选的命令行编辑器，尤其是当我在 Linux 机器上时，我在 Mac 上也经常使用它。

最近 Vim 中的一个[漏洞](https://nvd.nist.gov/vuln/detail/CVE-2019-12735)，被[发现](https://github.com/numirias/security/blob/master/doc/2019-06-04_ace-vim-neovim.md)。该漏洞与恶意使用 Vim 的 modelines 功能有关。