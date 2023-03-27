# Python 和 MySQL:pymysql 的一小步

> 原文：<https://medium.com/swlh/python-mysql-baby-steps-with-pymysql-89d62191f312>

在我目前的工作中，一个主要的焦点是数据处理和分析。源文件以 csv 格式从各种系统中获取，经过预处理，最后通过现成的 ETL 工具加载。

我决定尝试另一种数据分析方法，即利用 Jupyter Notebook 和 MySQL workbench 创建一种简单的方法，将原始 CSV 文件中的数据导入安全的数据库进行存储，并增加数据控制，同时利用 python 的强大功能和易用性来分析和预处理这些数据。