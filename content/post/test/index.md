---
title : '数据库'
date : '2025-03-14T22:36:35+08:00'
draft : false
slug: mysql-learning
categories:
    - 学习笔记
tags:
    - MySQL
---


# 数据库的相关定义

- 将大量数据保存起来，通过计算机加工而成的可以进行**高效访问的数据集合**称为数据库（DataBase，简称DB）。
- 用来管理数据库的计算机系统称为数据库管理系统（DBMS），例如MySQL。
- 从事管理和维护数据库管理系统的相关工作人员称为数据库管理员（DBA）。

## 表（table）

表是某种特定类型数据的结构化清单，即存储在表中的数据是同一种类型的数据或清单。数据库中每个表都有一个名字来标识自己，这个名字是唯一的。

> 虽然在一个数据库中不能两次使用相同的表名，但在不同的数据库中完全可以使用相同的表名



### 列（column）

表由列组成，列存储表中某部分的信息，所有的表都是由一个或多个列组成的。数据库中每个列都有相应的数据类型（datatype）。数据类型定义了列可以存储哪些数据种类。例如，如果列中存储的是数字，则相应的数据类型应该为数值类型。

> 数据类型限定了可存储在列中的数据种类（例如，防止在数值字段中录入字符值）。数据类型还帮助正确地分类数据，并在优化磁盘使用方面起重要的作用。

### 行（row）

表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

### 主键（primary key）

表中的每一行都应该有一列（或几列）可以**唯一标识**自己，即主键用来表示一个特定的行。没有主键，更新或删除表中特定行就极为困难，因为你不能保证操作只涉及相关的行，没有伤及无辜。

主键需要满足：

- 表格可以没有主键，但是最多只能拥有**一个**主键
- 主键的值必须是绝对唯一
- 主键的值不能为NULL，必须有对应的值
- 主键列中的值不允许修改和更新
- 主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）

## 什么是SQL

SQL 是结构化编程语言的缩写，是一种专门用来与数据库沟通的语言。SQL 不是某个特定数据库厂商专有的语言。绝大多数重要的 DBMS 支 持 SQL，所以学习此语言使你几乎能与所有数据库打交道。

# 检索数据

## SELECT语句

它的用途是从一个或多个表中检索信息。为了使用SELECT检索表数据，必须给出至少两条信息--想选择什么，以及从什么地方选择。

```mysql
SELECT name,age
FROM students;
```

上述语句利用SELECT语句从students表中检索两个分别名叫name和age的列。所需的列名写在SELECT关键字后，FROM关键字指出从哪个表中检索数据。

> 注意：多条SQL语句必须以分号（;）分割。SQL语句不区分大小写，通常对SQL关键字使用大写，而对列名和表名使用小写，这样做代码更易于阅读和调试。同样，在处理SQL语句时，其中所有空格都被忽略。

除了指定所需的列外，SELECT语句还可以检索所有的列而不必逐个列出它们。在实际列名的位置使用星号（*）通配符可以做到这点

```mysql
SELECT *
FROM students;
```

> 使用通配符有一个大优点，由于不明确指定列名，所以能检索出名字未知的列。但是除非必要，使用通配符检索不需要的列通常会降低检索速度和应用程序的性能。

### 检索不同的值

```mysql
SELECT DISTINCT class
FROM students;
```

第一行代码告诉DBMS只返回不同的（具有唯一性）的class行，如果使用 DISTINCT 关键字，它必须放在列名的前面。

> 注意：不能部分使用 DISTINCT。DISTINCT关键字作用于所有的列，不仅仅是跟在其后的那一列。

### 限制结果

SELECT 语句返回指定表中所有匹配的行，很可能是每一行。如果你只想返回第一行或一定数量的行，怎么办呢。注意！这一SQL语句在不同数据库的实现并不相同，下面以 MySQL 为例。

```mysql
SELECT *
FROM students
LIMIT 0,5;
```

上述语句表示查询students表的第1~5行，第一个参数 0 表示从第一行开始，第二参数 5 表示**一共查询**5条数据。

```mysql
# 等价于
SELECT *
FROM students
LIMIT 5;
```

### 排序数据

为了明确地排序用SELECT语句检索出的数据，可使用 ORDER BY 子句。ORDER BY 子句取一个或多个列的名字，据此对输出进行排序（默认升序排序）。

```mysql
SELECT name
FROM students
ORDER BY name;
```

> 注意：在指定一条 ORDER BY 子句时，应该保证它是SELECT语句中的最后一条子句。如果它不是最后的子句，将会出错。

**按多个列排序**

```mysql
SELECT id,score,name
FROM students
ORDER BY score,name;
```

上述语句先按成绩（score）排序，再按姓名（name）排序

```mysql
# 等价于
SELECT id,score,name
FROM students
ORDER BY 2,3
```

ORDER BY 2 表示按照SELECT清单中的第二个列 score 进行排序，ORDER BY 2，3 表示先按 score，再按name进行排序。

**指定排序方向**

数据排序不限于升序（从A到Z）排序，这只是默认的排序顺序，还可以使用ORDER BY子句进行降序（从Z到A）排序，为了实现降序排序，必须指定DESC关键字。

```mysql
SELECT id,score,name
FROM students
ORDER BY score DESC,name;
```

DESC 关键字只应用到直接位于其前面的列名。因此上述语句中score列将以降序排序，而name列（在每一个成绩内）仍然按标准的升序排序。

> 如果想在多个列上进行降序排序，必须对每一列指定DESC关键字

**注意：区分大小写和排序顺序**

在对文本性数据进行排序时，A与a相同吗？a位于B之前，还是Z之后？这些问题不是理论问题，其答案取决于数据库的设置方式，大多数数据库管理系统将A视为与a相同。

### 过滤数据

**使用 WHERE 子句**

数据库一般包含大量数据，很少需要检索表中的所有行。通常只会根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要指定搜索条件（过滤条件）

在 SELECT 语句中，数据根据 WHERE 子句中指定的搜索条件进行过滤。WHERE 子句在表名（FROM 子句）之后给出

```mysql
SELECT name, score
FROM students
WHERE score >= 60;
```

上述语句从 students 表中检索两个列，但不返回所有行，只返回 score 值大于等于60的行。

> 注意：在同时使用 ORDER BY 和 WHERE 子句时，应该让 ORDER BY 位于 WHERE 之后，否则将会发生错误。

**范围值检查**

要检索某个范围的值，可以使用 BETWEEN 操作符。其语法与其他 WHERE 子句的操作符略有不同，BETWEEN 匹配范围中所有的值，**包括指定的开始值和结束值**。

```mysql
SELECT name, score
FROM students
WHERE score BETWEEN 60 AND 100;
```

**空值检查**

在创建表时，表设计人员可以指定其中的列能否不包含值。在一个列不包含值时，称其包含空值 NULL。

> NULL：无值，它与字段包含0、空字符串或仅仅包含空格不同。

确定值是否为 NULL，不能简单地检查是否等于 NULL，SELECT 语句有一个特殊的 WHERE 子句，可用来检查具有 NULL 值的列。这个 WHERE 子句就是 IS NULL 子句。

```mysql
SELECT name
FROM students
WHERE score IS NULL;
```

上述语句返回没有成绩的学生姓名。

> 注意：通过过滤选择不包含指定值的所有行时，你可能希望返回含 NULL 值的行。但这是做不到的。因为 NULL 比较特殊，所以在进行匹配过滤或非匹配过滤时，不会返回这些结果。

**组合 WHERE 子句**

为了进行更强的过滤控制，SQL 允许给出多个 WHERE 子句。这些子句有两种使用方法，即以 AND 子句或 OR 子句的方式使用。

> **操作符：**用来联结或改变 WHERE 子句中的子句的关键字，也称为逻辑操作符

**AND操作符**

用在 WHERE 子句中的关键字，用来指示检索满足所有给定条件的行。

```mysql
SELECT id, score, name
FROM students
WHERE id = 1 AND score >= 60;
```

**OR操作符**

OR 操作符与 AND 操作符正好相反，它指示 DBMS 检索匹配任一条件的行。（事实上，许多 DBMS 在 OR WHERE 子句的第一个条件得到满足的情况下，就不再计算第二个条件了）

```mysql
SELECT id, score, name
FROM students
WHERE id = 1 OR score >= 60;
```

**求值顺序**


