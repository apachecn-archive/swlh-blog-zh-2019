# 通过 SSH 隧道远程访问 CloudSQL 数据库

> 原文：<https://medium.com/swlh/access-your-cloudsql-database-remotely-through-an-ssh-tunnel-18c0a70f2d2b>

仅具有私有 IP 的 CloudSQL 数据库仅可从**同一 VPC 和同一地区**内的资源**访问**。

*   您*无法*通过其私有 IP 地址从*另一个* VPC 网络访问云 SQL 实例，该网络使用 VPC 网络对等连接到 VPC 网络，并具有该云 SQL 实例的私有服务连接。
*   您*无法*使用云 VPN 隧道、基于实例的 VPN 或云互连从*另一个*网络访问其私有 IP 地址上的云 SQL 实例。此限制适用于…