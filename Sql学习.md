# Sql学习

任务：

- 多表查询

- avg

  ```
  SELECT AVG(OrderPrice) AS average FROM Orders
  ```

- group by 多个相同的名字合并为一条

- having 在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。



### 查看数据库和表

```mysql
show databases;
show tables;
```



### 创建数据库和表

```mysql
create database mysqltrain;
create table `student`
(
    `sid` INT(10),
    `sname` VARCHAR(20),
    `sage` INT(20),
    `ssex` VARCHAR(20)
)engine=innodb defualt charset=utf8 #要添加，要不添加中问会失败

#利用sql语句进行bai修改，举例说du明：
ALTER TABLE `daotest` DEFAULT CHARACTER SET utf8;
#该命令用于将表回test的编码方式改为utf8；答
ALTER TABLE `test` CHANGE `name` `name` VARCHAR(36) CHARACTER SET utf8 NOT NULL; 
#该命令用于将表test中name字段的编码方式改为utf8
```



### 删除表

```mysql
drop table student;
```



### 插入数据

```mysql
#
#单条插入
insert into student (sid,sname,sage,ssex) value(0,"张三",12,"男")

#多条插入1
insert into student (sid,sname,sage,ssex) values(1,"李四",15,"女"),
(2,"王五",14,"男")
#多条插入2
insert into student (sid,sname,sage,ssex) 
select 3,"刘一",13,"男" union all
select 4,"钱二",16,"女"

#如果插入的记录是数字的话要在数字的逗号后面加N:
insert into Student 
select 1,N'刘一',18,N'男' union all
select 2,N'钱二',19,N'女';
#简单的说就是为了更方便的处理拉丁（比如英文）和非拉丁（例如中文，日本）在程序中的兼容

#复制旧表中的信息添加到新表中
insert into student1 select * from student2 where sage=18;

```



问题： 

1. 查询“001”课程比“002”课程成绩高的所有学生的学号； 

   ```mysql
   select a.sid from 
   (select sid,score from sc where cid=1)a,
   (select sid,score from sc where cid=2)b
   where a.score > b.score and a.sid = b.sid;
   ```

   

2. 查询平均成绩大于60分的同学的学号和平均成绩； 

   

3. 查询所有同学的学号、姓名、选课数、总成绩； 

   

4. 查询姓“李”的老师的个数； 

   

5. 查询没学过“叶平”老师课的同学的学号、姓名； 

   

6. 查询学过“001”并且也学过编号“002”课程的同学的学号、姓名； 

   

7. 查询学过“叶平”老师所教的所有课的同学的学号、姓名； 

   

8. 查询课程编号“002”的成绩比课程编号“001”课程低的所有同学的学号、姓名；

    

9. 查询所有课程成绩小于60分的同学的学号、姓名； 

   

10. 查询没有学全所有课的同学的学号、姓名；

    

11. 查询至少有一门课与学号为“1001”的同学所学相同的同学的学号和姓名；

     

12. 查询至少学过学号为“001”同学所有一门课的其他同学学号和姓名； 

    

13. 把“SC”表中“叶平”老师教的课的成绩都更改为此课程的平均成绩； 

    

# 笔记：

```mysql
insert into student (, , , ) value( , , , )
delete from student where id=111;
update stduent set sname= , sage=  where id= 
```



## SQL DML 和 DDL

可以把 SQL 分为两个部分：数据操作语言 (DML) 和 数据定义语言 (DDL)。

DML 部分：

- *SELECT*
- *UPDATE* 
- *DELETE* 
- *INSERT INTO*

DDL 语句:

- *CREATE DATABASE* - 创建新数据库
- *ALTER DATABASE* - 修改数据库
- *CREATE TABLE* - 创建新表
- *ALTER TABLE* - 变更（改变）数据库表
- *DROP TABLE* - 删除表
- *CREATE INDEX* - 创建索引（搜索键）
- *DROP INDEX* - 删除索引



## DISTINCT 语句

重复的值只出现一次



## AND 和 OR 运算符

我们也可以把 AND 和 OR 结合起来（使用圆括号来组成复杂的表达式）:

```mysql
SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William')
AND LastName='Carter
```



## ORDER BY 语句

ORDER BY 语句用于根据指定的列对结果集进行排序。

ORDER BY 语句默认按照升序对记录进行排序。

如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。



**DESC 为逆序， ASC 正序**

以逆字母顺序显示公司名称，并以数字顺序显示顺序号：

```mysql
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```

### 结果：

| Company  | OrderNumber |
| :------- | :---------- |
| W3School | 2356        |
| W3School | 6953        |
| IBM      | 3532        |
| Apple    | 4698        |





## TOP 实例

现在，我们希望从上面的 "Persons" 表中**选取头两条记录**。

我们可以使用下面的 SELECT 语句：

```
SELECT TOP 2 * FROM Persons
```



## LIKE 操作符

以 "N" 开始的城市里的人`WHERE City LIKE 'N%'`

以 "g" 结尾的城市里的人`WHERE City LIKE '%g'`

包含 "lon" 的城市里的人`WHERE City LIKE '%lon%'`

*不包含* "lon" 的城市里的人`WHERE City NOT LIKE '%lon%'`



## SQL 通配符

| 通配符                     | 描述                   |
| :------------------------- | :--------------------- |
| %                          | 替代一个或多个字符     |
| _                          | 仅替代一个字符         |
| [charlist]                 | 字符列中的任何单一字符 |
| [^charlist]或者[!charlist] | 不在字符列中的任何     |

选取居住的城市以 "A" 或 "L" 或 "N" 开头的人：`WHERE City LIKE '[ALN]%'`

选取居住的城市**不**以 "A" 或 "L" 或 "N" 开头的人：`WHERE City LIKE '[!ALN]%'`



## IN 操作符

IN 操作符允许我们在 WHERE 子句中规定多个值。

从上表中选取姓氏为 Adams 和 Carter 的人：`WHERE LastName IN ('Adams','Carter')`





## BETWEEN 操作符

左闭右开

以字母顺序显示介于 "Adams"（包括）和 "Carter"（不包括）之间的人：

`WHERE LastName BETWEEN 'Adams' AND 'Carter'`



## 不同的 SQL JOIN

**注释：**在某些数据库中， LEFT JOIN 称为 LEFT **OUTER** JOIN（还有RIGHT、FULL）。

- JOIN: 如果表中有至少一个匹配，则返回行（与下相同）

- INNER JOIN：在表中存在至少一个匹配时，INNER JOIN 关键字返回行。

- LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行

  右表，有符合的就返回，没有的话也无所谓，左表照样全部输出（下同）

- RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行

- FULL JOIN: 只要其中一个表中存在匹配，就返回行

```mysql
FROM Persons
RIGHT JOIN Orders
ON Persons.Id_P=Orders.Id_P # join用的
```



## UNION 操作符 和 UNION ALL 操作符

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

### SQL UNION 语法

```
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

### SQL UNION ALL 语法

```
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```

另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

**两结果拼起来**





## SELECT INTO 语句

从一个表中选取数据，然后把数据插入另一个表中。

### SQL SELECT INTO 实例 - 制作备份复件

下面的例子会制作 "Persons" 表的备份复件：

```
SELECT *
INTO Persons_backup
FROM Persons
```



## SQL 约束

约束：

- NOT NULL 不为空

- UNIQUE 唯一

  ```mysql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  UNIQUE (Id_P)
  )
  
  #当表已被创建时，如需在 "Id_P" 列创建 UNIQUE 约束，请使用下列 SQL
  ALTER TABLE Persons ADD UNIQUE (pId)
  
  #撤销 UNIQUE 约束，请使用下面的 SQL：
  ALTER TABLE Persons DROP INDEX uc_PersonID
  ```

- PRIMARY KEY 主键

  已创建后，同上

- FOREIGN KEY 外键

  表1的主键若在表二也存在，那就是表2的外键

  FOREIGN KEY 约束用于预防破坏表之间连接的动作。

  FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

  ```mysql
  CREATE TABLE Orders
  (
  oid int NOT NULL,
  OrderNo int NOT NULL,
  pid int,
  PRIMARY KEY (oid),
  FOREIGN KEY (pid) REFERENCES Persons(pid)
  )
  
  # 对已有的添加
  ALTER TABLE Orders
  ADD FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
  
  # 撤销
  ALTER TABLE Orders DROP FOREIGN KEY fk_PerOrders
  ```

- CHECK

  约束用于限制列中的值的范围。

  ```mysql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CHECK (Id_P>0)
  )
  
  #
  ALTER TABLE Persons ADD CHECK (Id_P>0)
  #
  ALTER TABLE Persons
  DROP CHECK chk_Person
  ```

- DEFAULT 默认

  ```mysql
  City varchar(255) DEFAULT 'Sandnes'
  ```

  

# CREATE INDEX 语句

**表中创建索引。**

### 索引

以==便更加快速高效地查询数据。==

用户无法看到索引，它们只能被==用来加速搜索/查询。==

**注释：**==更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在经常被搜索的列（以及表）上面创建索引。==



## CREATE INDEX 语法

创建一个简单的索引。允许使用重复的值：

```mysql
CREATE INDEX index_name
ON table_name (column_name)
```

### CREATE UNIQUE INDEX 语法

创建一个唯一的索引。唯一的索引意味着两个行不能拥有相同的索引值。

```mysql
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

### CREATE INDEX 实例

本例会创建一个简单的索引，名为 "PersonIndex"，在 Person 表的 LastName 列：

```mysql
CREATE INDEX PersonIndex
ON Person (LastName) 
```

如果您希望以降序索引某个列中的值，您可以在列名称之后添加保留字 *DESC*：

```mysql
CREATE INDEX PersonIndex
ON Person (LastName DESC) 
```

假如您希望索引不止一个列，您可以在括号中列出这些列的名称，用逗号隔开：

```mysql
CREATE INDEX PersonIndex
ON Person (LastName, FirstName)
```



## TRUNCATE TABLE 语句

## DROP 语句



## ALTER TABLE 语法

如需在表中添加列，请使用下列语法:

```mysql
ALTER TABLE table_name
ADD column_name datatype
```

要删除表中的列，请使用下列语法：

```mysql
ALTER TABLE table_name 
DROP COLUMN column_name
```

**注释：**某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。





## AUTO INCREMENT 字段

自动地创建主键字段的值

```mysql
CREATE TABLE Persons
(
P_Id int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
City varchar(255),
PRIMARY KEY (P_Id)
)
```





## SQL VIEW（视图）

**视图是可视化的表。**

**CREATE VIEW** <视图名> **AS** <SELECT语句>

### 什么是视图？

在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。

视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。（将结果集合并成像一张表）

**注释：**数据库的设计和结构不会受到视图中的函数、where 或 join 语句的影响。



## SQL IS NULL

我们如何仅仅选取在 "Address" 列中带有 NULL 值的记录呢？

我们必须使用 IS NULL 操作符：

```
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL
```





## DATE 类型

| 数据类型    | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| DATE()      | 日期。格式：YYYY-MM-DD注释：支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | 日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS注释：支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | 时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的描述来存储。格式：YYYY-MM-DD HH:MM:SS<br />注释：支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS 注释：支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。注释：4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

问题：date 与 timestamp 的区别





## ROUND() 函数

ROUND 函数用于把数值字段舍入为指定的小数位数。四舍五入

### SQL ROUND() 语法

```
SELECT ROUND(column_name,decimals) FROM table_name
```

| 参数        | 描述                         |
| :---------- | :--------------------------- |
| column_name | 必需。要舍入的字段。         |
| decimals    | 必需。规定要返回的小数位数。 |

```mysql
SELECT ProductName, ROUND(price,0) as UnitPrice FROM Products
```



## NOW() 函数

```
SELECT NOW() FROM table_name
```



## FORMAT() 函数

FORMAT 函数用于对字段的显示进行格式化。

### SQL FORMAT() 语法

```
SELECT FORMAT(column_name,format) FROM table_name
```

| 参数        | 描述                   |
| :---------- | :--------------------- |
| column_name | 必需。要格式化的字段。 |
| format      | 必需。规定格式。       |

```mysql
SELECT ProductName,FORMAT(Now(),'YYYY-MM-DD') as PerDate
FROM Products
```

