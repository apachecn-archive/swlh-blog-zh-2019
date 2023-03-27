# SQL 基础知识

> 原文：<https://medium.com/swlh/basics-of-sql-7f1e72772e63>

SQL 代表结构化查询语言，用于与数据库通信。本文将介绍 SQL 的一些基础知识！

1.创建数据库

```
CREATE DATABASE *database_name*CREATE DATABASE *hero_database*
```

2.创建表格

```
CREATE TABLE table_name (
   id INTEGER PRIMARY KEY,
   name TEXT,
   age INTEGER
);CREATE TABLE heroes(
   id INTEGER PRIMARY KEY,
   name TEXT,
   age INTEGER,
   ability TEXT
);
```

3.限制

在创建(或稍后更改)表时使用，以限制放入其中的数据类型。有列级和表级约束。

```
CREATE TABLE table_name (
   id INTEGER PRIMARY KEY,
   name TEXT constraint,
   age INTEGER constraint
);CREATE TABLE heroes (
   id INTEGER PRIMARY KEY,
   name TEXT NOT NULL,
   age INTEGER NOT NULL,
   ability TEXT NOT NULL
);
```

一些常见的限制是:

```
NOT NULL
# column cannot have a null valueUNIQUE
# all values in column are differentPRIMARY KEY
# combination of NOT NULL and UNIQUEFOREIGN KEY
# uniquely identifies row in another tableCHECK
# ensures all values in column satisfy a specific conditionDEFAULT
# sets a default value for the column when no value is specifiedINDEX
#  Used to create and retrieve data from the database quickly
```

4.添加列

```
ALTER TABLE table_name ADD COLUMN column_name TEXT;ALTER TABLE heroes ADD COLUMN gender TEXT;
```

5.翻桌

```
DROP TABLE table_nameDROP TABLE heroes
```

6.挑选

```
SELECT column_name FROM table_nameSELECT ability FROM heroes
```

7.明显的

仅选择唯一的值。

```
SELECT DISTINCT column_name FROM table_nameSELECT DISTINCT ability FROM heroes
```

8.在哪里

基于指定的条件选择数据。

```
SELECT column_name FROM table_name WHERE conditionSELECT name FROM heroes WHERE name = "Batman"
```

也可以使用 AND 和 OR 来指定更多的条件。

```
SELECT name FROM heroes WHERE name = "Batman" OR "Superman"
```

9.数数

给出满足指定条件的记录数量。

```
SELECT COUNT(column_name) FROM table_name WHERE conditionSELECT COUNT(ability) FROM heroes WHERE ability = "ESP"
```

10.以...排序

按列值的升序(默认)或降序对结果进行排序。

```
SELECT "column_name" FROM "table_name" WHERE "condition" ORDER BY column_name ASC|DESCSELECT name FROM heroes WHERE age > 16 ORDER BY age DESC
```

11.分组依据

按指定的列对结果进行分组。

```
SELECT COUNT(column_name) FROM table_name GROUP BY column_nameSELECT COUNT(ability) FROM heroes GROUP BY ability
```

12.插入

将数据插入表格。

```
INSERT INTO table_name (name, age, gender, ability) VALUES ("name_value", age_value, "gender_value")INSERT INTO heroes (name, age, gender, ability) VALUES ("Spiderman", 18, "male", "spider things")
```

13.更新

更新表中数据的信息。

```
UPDATE table_name SET name = "new_name" WHERE name = "old_name";UPDATE heroes SET name = "spoderman" WHERE name = "Spiderman";
```

14.删除

从数据库中删除一行。

```
DELETE FROM table_name WHERE conditionDELETE FROM heroes WHERE name="spoderman"
```

15.选择进入

将数据从一个表复制到另一个表中。

```
SELECT column_name(s) INTO new_table FROM old_table WHERE conditionSELECT name INTO people FROM heroes WHERE name = "Batman"
```

16.存储过程

因为当你有运行的公共代码时，你可以存储它并调用它。

要储存它:

```
CREATE PROCEDURE *procedure_name*
   AS
*sql_statement*
   GO;CREATE PROCEDURE *select_psychics*
   AS
*SELECT * FROM heroes WHERE ability = "ESP"*
   GO;
```

称之为:

```
EXEC *procedure_name*
```

17.连接

有四种类型的连接。

*   内部联接-当两个表中至少有一个匹配项时，返回所有行
*   左连接-返回左表中的所有行，以及右表中的匹配行
*   右连接-返回右表中的所有行，以及左表中的匹配行
*   完全连接-当其中一个表中存在匹配项时，返回所有行

```
SELECT column_name FROM table_name_1 INNER JOIN table_name_2 ON table_name_1.column_name = table_name_2.column.name
```

通常一个表有一列包含另一个表的外键，就像如果有一个图书表和一个作者表，图书表可能有一列 author _ ids。然后将该列连接到另一个表的 id 列上。

您可以通过执行以下操作来查询连接的表:

```
SELECT heroes.name, people.name 
FROM
heroes
INNER JOIN 
people 
ON 
heroes.id = people.heroes_id
```

这些是关于如何创建表、从表中查询信息以及更改表中内容的基础知识。无论是自己做还是用别人的，祝你在使用表格时好运！

*   *编辑

有一个在面试中难倒了我，显然是所谓的自我加入。

假设你有一张这样的桌子:

```
EMPLOYEEemp_id | emp_name | emp_supervisor_id
00001    Ed              00003
00002    Edd             00003
00003    Eddy            00004
00004    Fred               -
00005    Fredd           00004
00006    Freddy          00004
```

这里弗雷德是大老板。他的下属是弗雷德、弗雷迪和艾迪。Eddy 也是 Edd 和 Ed 的老板。你要返回一张列有雇员名单及其雇主姓名的表格。但是对于每一行，您看不到员工顾问的姓名，只能看到他们顾问的 id 号。要获得顾问的名称，您需要使用自联接。

```
SELECT a.emp_id as “Emp_ID”, a.emp_name AS “Employee Name”
SELECT b.emp_id as “Sup_ID”, b.emp_name AS “Supervisor Name”
FROM employee a, employee b
WHERE a.emp_supervisor_id = b.emp_id
```

在这里，我们在 emp_supervisor_id 和 emp_i 重叠的地方连接一个表。然后，在第一行中，您将获得所有雇员，并获得两列，雇员 id(别名为 Emp_ID)和雇员姓名(别名为 Employee Name)。然后对于第二个表，我们得到雇员 ID(这次别名为 Sup_ID)和雇员姓名(别名为主管姓名)。最终结果将是:

```
Emp_ID | Employee Name | Sup_ID | Supervisor Name
00001    Ed              00003         Eddy
00002    Edd             00003         Eddy
00003    Eddy            00004         Fred
00004    Fred               -
00005    Fredd           00004         Fred
00006    Freddy          00004         Fred
```