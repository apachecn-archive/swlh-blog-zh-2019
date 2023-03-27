# 像使用 Python 中的本地文件系统一样使用 S3

> 原文：<https://medium.com/swlh/using-s3-just-like-a-local-file-system-in-python-497737783f11>

![](img/acaa0827a496aadf2346c2224021fbfd.png)

“S3 just like a local drive, in Python”

有一个很酷的 Python 模块叫做 [s3fs](https://github.com/dask/s3fs) ，它可以“挂载”S3，所以你可以对文件使用 POSIX 操作。

为什么您会关心 POSIX 操作呢？因为 python 也实现了它们。因此，如果您碰巧正在运行 python 应用程序，可以通过以下方式将内容写入本地文件:

> 开(路径，“w”)为 f:
> 
> 写入(f)