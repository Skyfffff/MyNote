#  Mysql【基础篇】

## SQL分类

| 分类 | 全称                       | 说明                                                  |
| ---- | -------------------------- | ----------------------------------------------------- |
| DDL  | Data Definition Language   | 数据定义语言,用来定义数据库对象（数据库,表,字段）     |
| DML  | Data Manipulation Language | 数据操作语言,用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language        | 数据查询语言,用来查询数据库中的表记录                 |
| DCL  | Data Contol Language       | 数据控制语言,用来创建数据库用户、控制数据库的访问权限 |

## 数据类型

### 整数类型

| 类型           | 大小（字节） | 有符号数取值范围                | 无符号数取值范围   | 说明     |
| -------------- | ------------ | ------------------------------- | ------------------ | -------- |
| TINYINT        | 1            | (-128, 127)                     | (0, 255)           | 超小整数 |
| SMALLINT       | 2            | (-32 768, 32 767)               | (0, 65 535)        | 小整数   |
| MEDIUMINT      | 3            | (-8 388 608, 8 388 607)         | (0, 16 777 215)    | 中等整数 |
| INT 或 INTEGER | 4            | (-2 147 483 648, 2 147 483 647) | (0, 4 294 967 295) | 整数     |
| BIGINT         | 8            | (-263, 263-1)                   | (0, 264-1 )        | 大整数   |

### 小数类型

| 类型             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| FLOAT(size, d)   | 单精度浮点数类型,4 个字节大小。size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。 |
| FLOAT(p)         | 单精度浮点数类型,参数 p 用来决定使用 FLOAT 类型还是 DOUBLE 类型：如果 p 的取值介于 0 和 24 之间,那么数据类型将变成 FLOAT()；如果 p 的取值介于 25 和 53 之间,那么数据类型将变成 DOUBLE()。 |
| DOUBLE(size, d)  | 双精度浮点数类型,8个字节大小。size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。 |
| DECIMAL(size, d) | 定点数类型,size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。size 的最大值是 65,默认值是 10；d 的最大取值是 30,默认值是 0。 |
| DEC(size, d)     | 等价于 DECIMAL(size, d)。                                    |

### 字符串类型

| 类型                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| CHAR(size)                 | 用于表示固定长度的字符串,该字符串可以包含数字、字母和特殊字符。size 的大小可以是从 0 到 255 个字符,默认值为 1。 |
| VARCHAR(size)              | 用于表示可变长度的字符串,该字符串可以包含数字、字母和特殊字符。size 的大小可以是从 0 到 65535 个字符。 |
| TINYTEXT                   | 表示一个最大长度为 255（2 8-1）的字符串文本。                |
| TEXT(size)                 | 表示一个最大长度为 65,535（216-1）的字符串文本,也即 64KB。   |
| MEDIUMTEXT                 | 表示一个最大长度为 16,777,215（224-1）的字符串文本,也即 16MB。 |
| LONGTEXT                   | 表示一个最大长度为 4,294,967,295（232-1）的字符串文本,也即 4GB。 |
| ENUM(val1, val2, val3,...) | 字符串枚举类型,最多可以包含 65,535 个枚举值。插入的数据必须位于列表中,并且只能命中其中一个值；如果不在,将插入一个空值。 |
| SET( val1,val2,val3,....)  | 字符串集合类型,最多可以列出 64 个值。插入的数据可以命中其中的一个或者多个值,如果没有命中,将插入一个空值。 |

### 日期时间类型

| 类型           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| DATE           | 日期类型,格式为 YYYY-MM-DD,取值范围从 '1000-01-01' 到 '9999-12-31'。 |
| DATETIME(fsp)  | 日期和时间类型,格式为 YYYY-MM-DD hh:mm:ss,取值范围从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'。 |
| TIMESTAMP(fsp) | 时间戳类型,它存储的值为从 Unix 纪元（'1970-01-01 00:00:00' UTC）到现在的秒数。TIMESTAMP 的格式为 YYYY-MM-DD hh:mm:ss,取值范围从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC。 |
| TIME(fsp)      | 时间类型,格式为 hh:mm:ss,取值范围从 '-838:59:59' 到 '838:59:59'。 |
| YEAR           | 四位数字的年份格式,允许使用从 1901 到 2155 之间的四位数字的年份。此外,还有一个特殊的取值,就是 0000。 |

### 二进制类型

| 类型            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| BIT(size)       | 二进制位（Bit）类型,位数由 size 参数指定；size 的大小从 1 到 64,默认值为 1。 |
| BINARY(Size)    | 等价于 CHAR() 类型,但是存储的是二进制形式的字节串。size 参数以字节（Byte）为单位指定列的长度,默认值为1。 |
| VARBINARY(Size) | 等价于 VARCHAR() 类型,但是存储的是二进制形式的字节串。size 参数以字节（Byte）为单位指定列的最大长度。 |
| TINYBLOB        | 存储较小的二进制数据,最多可容纳 255 (28-1)个字节。           |
| BLOB(size)      | 用来储存二进制数据,最多可以容纳 65,535（216-1）个字节,也即 64KB。 |
| MEDIUMBLOB      | 存储中等大小的二进制数据,最多可以容纳 16,777,215（224-1）字节,也即 16MB。 |
| LONGBLOB        | 存储较大的二进制数据,最多可容纳 42,94,967,295（232-1）字节,也即 4GB。 |

## Data Definition Language

### 数据库操作

```sql
#查询所有数据库
SHOW DATABASES; 

#查询当前数据库
SELECT DATABASES(); 

#创建数据库
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]

#删除数据库
DROP DATABASE[IF EXISTS]数据库名

#使用数据库
USE 数据库名
```

### 表操作

```sql
#查询所有表
SHOW TABLES;

#查询表结构
DESC 表名;

#查询建表语句
SHOW CREATE TABLE 表名;

#删除表
DROP TABLE 表名;

#创建表
create table user_test(
    -> id int comment '编号',
    -> name varchar(50) comment '姓名',
    -> age int comment '年龄',
    -> gender varchar(1) comment '性别'
    -> ) comment '用户表';
```

### 字段操作

```sql
#添加字段
ALTER TABLE 表名 ADD 字段名 类型 [COMMENT 注释] [约束]

#修改数据类型
ALTER TABLE 表名 MODIFY 字段名 新的数据类型

#删除列
ALTER TABLE 表名 DROP COLUMN 字段名

#修改字段名和字段类型
ALTER TABLE 表名 CHANGE 旧的字段名 新的字段名 类型 [COMMENT 注释] [约束]
```

## Data Manipulation Language

### 插入数据

```sql
#给指定表中的指定字段添加数据
INSERT INTO 表名 (字段名1,字段名2,...) VALUES (值1,值2,...);

#给指定表中的所有字段添加数据
INSERT INTO 表名 VALUES (值1,值2,...);

#批量添加数据
INSERT INTO 表名 (字段名1,字段名2,...) VALUES (值1,值2,...),(值1,值2,...);
INSERT INTO 表名 VALUES (值1,值2,...),(值1,值2,...);
```

### 修改数据

```sql
#修改指定字段值
UPDATA 表名 SET 字段名1 = 值1,字段名1 = 值1 [WHERE 条件语句];

#修改所有字段值！！！
UPDATA 表名 SET 字段名1 = 值1,字段名1 = 值1;
```

### 删除数据

```sql
#删除指定数据
DELETE FROM 表名 [WHRER 条件];

#删除所有数据
DELETE FROM 表名;
```

## Data Query Language

### 基础查询

```sql
#查询指定字段数据
SELECT 字段名1,字段名2 FROM 表名;

#查询表中所有数据
SELECT * from 表名;

#查询指定字段数据(不重复)
SELECT DISTINCT 字段名1,字段名2 from 表名;

#查询框架
SELECT 字段列表
FROM 表名
WHERE 条件
GROUP BY 分组字段
HAVING 分组过滤条件
ORDER BY 排序字段
LIMIT 分页参数

执行顺序：
FROM
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
LIMIT
```

### 条件查询

| 操作符           | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| =                | 等号，检测两个值是否相等，如果相等返回true                   |
| <> 或 !=         | 不等于，检测两个值是否相等，如果不相等返回true               |
| >                | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true |
| <                | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true |
| >=               | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true |
| <=               | 小于等于号，检测左边的值是否小于于或等于右边的值, 如果左边的值小于或等于右边的值返回true |
| BETWEEN...AND... | 在某个范围之内(包含边界)                                     |
| IN(...)          | 在in之后的列表中的值，多选一                                 |
| LIKE 占位符      | 模糊匹配(_匹配单个字符，%匹配任意个字符)                     |
| IS NULL          | 是NULL                                                       |
| AND 或 &&        | 并且                                                         |
| OR  或 \|\|      | 或者                                                         |
| NOT 或 ！        | 非                                                           |

```sql
#条件查询
SELECT 字段名1，字段名2，... FROM 表名 WHERE 条件

例子：
select * 
from user_test 
where age <= 18;

select * 
from user_test 
where age between 15 and 18;

select *
from user_test
where age in(17,18,19);

#查询所有名字是四个字符的数据
select *
from user_test
where name like '____';

#查询年龄以0结尾的数据
select *
from user_test
where age like '%0';
```

### 聚合查询

```sql
例子：
#统计所有name字段的数目
select 
count(name) 
from user_test;

#统计表中age的平均数
select 
avg(age) 
from user_test;

#统计表中age的最大值
select 
max(age) 
from user_test;

#统计表中age的最小值
select 
min(age) 
from user_test;

#统计性别为男性的年龄总和
select 
sum(age) 
from user_test 
where gender = '1' or gender = '男';
```

### 分组查询

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 字段名 [HAVING 分组过滤条件]
#PS：where是分组之前过滤，having是分组之后过滤

例子：
#统计男女分别有多少条数据
select gender, count(*)
from user_test
group by gender;

#统计男女分别的平均年龄
select gender, avg(age)
from user_test
group by gender;

#统计年龄小于等于30岁的数据男女分别有多少条，并且保留统计结果大于5的记录
select gender, count(*)
from user_test
where age <= 30
group by gender
having count(*) > 5;
```

###  排序查询

```sql
SELECT 字段名 FROM 表名 ORDER BY 字段名 排序方式
#PS：排序方式：asc 升序 desc降序

#根据年龄进行降序排序
select *
from user_test
order by age desc;

#先根据name进行升序排序，在根据age进行降序排序
select *
from user_test
order by name asc,age desc;
```

### 分页查询

```sql
SELECT 字段列表 FROM 表名 LIMIT 其实索引，查询记录数；
#PS：起始索引=（查询页码-1）*每页记录数

#查询其实索引为0，每页10条数据
select *
from user_test
limit 0,10;
```

### 多表查询

#### 内连接查询

> 内连接查询是两张表的交集部分

```sql
#隐式内连接
SELECT 字段列表 FROM 表1，表2 WHERE 条件...
例子：
select *
from table2,
     dept
where dept.id = table2.dept_id;

#显式内连接
SELECT 字段列表 FROM 表1 [INNER]JOIN 表2 ON 连接条件...
例子：
select *
from table2
         inner join dept on table2.dept_id = dept.id;
```

#### 外连接查询

> 相当于查询表1（左表）的所有数据 包含 表1和表2交集部分的数据

```sql
#左外连接
SELECT 字段列表 FROM 表1 LEFT [OUTER]JOIN 表2 ON 条件
例子：
select table2.*, dept.*
from table2
         left outer join dept on table2.dept_id = dept.id;
```

> 相当于查询表2（右表）的所有数据 包含 表1和表2交集部分的数据

```sql
#右外连接
SELECT 字段列表 FROM 表1 RIGHT [OUTER]JOIN 表2 ON 条件
例子： 
select dept.*, table2.*
from table2
         right outer join dept on table2.dept_id = dept.id;
```

#### 自连接查询

> 自连接可以是内连接也可以是外连接

```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...
```

#### 联合查询

```sql
#联合查询
SQL语句1
UNION[ALL]
SQL语句2;

例子：去重
select * from table2 where age < 50
union
select * from table2 where gender = 'F';

例子：不去重
select * from table2 where age < 50
union all
select * from table2 where gender = 'F';
```

#### 子查询

- 标量子查询

> 子查询结果返回单个值

```sql
#查询department为aa的table2表的所有数据
select *
from table2
where dept_id = (select id from dept where department = 'aa');
```

- 列子查询

> 子查询结果为一列

```sql
#查询department为aa或的table2表的所有数据
select *
from table2
where dept_id in (select id from dept where department = 'aa' or department = 'bb');
```

- 行子查询

> 子查询结果为一行

```sql
select *
from emp
where (salary, managerid) = (select salary, managerid from emp where name = 'sky');
```

- 表子查询

> 子查询结果为多行多列

```sql
select *
from emp
where (job, salary) in (select job, salary from emp where name = 'sky' or name = 'shabo');
```

##   Data Contol Language

### 用户管理

```sql
#创建用户 %代表所有主机都可以访问
create user 'sky'@'localhost' identified by '123456';
create user 'sky'@'%' identified by '123456';

#修改用户密码
set password for 'sky6'@'localhost' = password('666');

#删除用户
drop user 'sky'@'localhost';
```

### 权限控制

```sql
#查询权限
SHOW GRANTS FOR '用户名'@'主机名';
show grants for 'sky'@'localhost';

#授予权限
GRANT ALL ON 数据库名.表名 TO '用户名'@'主机名';
grant all on test1.* to 'sky'@'localhost';

#撤销权限
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
revoke all on test1.* from 'sky'@'localhost';
```

## 约束

### 关键字约束

| 约束               | 描述                                                   | 关键字      |
| ------------------ | ------------------------------------------------------ | ----------- |
| 非空约束           | 现在字段数据不能为null                                 | NOT NULL    |
| 唯一约束           | 保证字段所有数据唯一、不重复                           | UNIQUE      |
| 主键约束           | 主键是一行数据的唯一标识，要求非空且唯一               | PRIMARY KEY |
| 默认约束           | 保存数据是，如果未指定该字段的值，则采用默认值         | DEFAULT     |
| 检查约束（8.0.16） | 保证字段值满足某一个条件                               | CHECK       |
| 外键约束           | 用来让两张表数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |

```sql
create table table2 (
    id int primary key auto_increment comment 'id',
    name varchar(20) not null unique comment '姓名',
    age int comment '年龄',
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
);
```

### 外键约束

```sql
#添加外键
ALTER TABLE 表名 
ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
ON UPDATE 行为 ON DELETE 行为
例子：
alter table table2 add constraint fk_table2_dept_id foreign key (dept_id) references dept(id);

CONSTRAINT子句允许您为外键约束定义约束名称。如果省略它，MySQL将自动生成一个名称。
FOREIGN KEY子句指定子表中引用父表中主键列的列。您可以在FOREIGN KEY子句后放置一个外键名称，或者让MySQL为您创建一个名称。 请注意，MySQL会自动创建一个具有foreign_key_name名称的索引。
REFERENCES子句指定父表及其子表中列的引用。 在FOREIGN KEY和REFERENCES中指定的子表和父表中的列数必须相同。

#删除外键
ALTER TABLE 表名
DROP FOREIGN KEY 外键名称
```

### 行为补充

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新 |
| RESTRICT    | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新 |
| CASCADE     | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录 |
| SET NULL    | 当在父表中删除对应记录是，首先检查该记录是否有对应外键，如果有则设置子表中该外键的值为null |
| SET DEFAULT | 父表有变更时，子表将外键列设置成一个默认的值（innodb不支持） |

```sql
#添加外键
ALTER TABLE 表名 
ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
ON UPDATE 行为 ON DELETE 行为
```

## 事物

### 事物特性

| 特性       | 描述                                                     |
| ---------- | -------------------------------------------------------- |
| **原子性** | 是不可分割的最小的操作单位，要么同时成功，要么同时失败。 |
| **持久性** | 当事务提交或回滚后，数据库会持久化的保存数据。           |
| **隔离性** | 多个事务之间。相互独立。                                 |
| **一致性** | 多个事务之间。相互独立。                                 |

### 事物操作

- 方式1

```sql
#设置当前会话取消自动提交事物
set @@autocommit = 0;

update account set money = money - 500 where name = '张三';
update account set money = money + 500 where name = '李四';

#提交事物
commit ;

#事物回滚（清空缓存区）
rollback ;
```

- 方式2

```sql
#开始事物
start transaction ;

update account set money = money - 500 where name = '张三';
update account set money = money + 500 where name = '李四';
#提交事物
commit ;

#事物回滚（清空缓存区）
rollback ;
```

### 并发事物问题

| 问题           | 描述                                   |
| -------------- | -------------------------------------- |
| **脏读**       | 一个事务读取了另一个事务未提交的数据。 |
| **不可重复读** | 一个事务前后两次读取的同一数据不一致。 |
| **幻读**       | 一个事务两次查询的结果集记录数不一致。 |

### 事务隔离级别

| 隔离级别                     | 脏读 | 不可重复读 | 幻读 | 隔离性 | 并发性 |
| :--------------------------- | :--: | :--------: | :--: | :----: | :----: |
| 顺序读（SERIALIZABLE）       |  N   |     N      |  N   |  最高  |  最低  |
| 可重复读（REPEATABLE READ）  |  N   |     N      |  Y   |        |        |
| 读以提交（READ COMMITTED）   |  N   |     Y      |  Y   |        |        |
| 读未提交（READ UNCOMMITTED） |  Y   |     Y      |  Y   |  最低  |  最高  |

```sql
#查看事物隔离界别
select @@transaction_isolation;

#设置事物隔离级别		
set session transaction isolation level repeatable read ;
```



## 内置函数

### 字符串函数

| 函数                                  | 描述                                                         | 实例                                                         |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ASCII(s)                              | 返回字符串 s 的第一个字符的 ASCII 码。                       | 返回 CustomerName 字段第一个字母的 ASCII 码：`SELECT ASCII(CustomerName) AS NumCodeOfFirstChar FROM Customers;` |
| CHAR_LENGTH(s)                        | 返回字符串 s 的字符数                                        | 返回字符串 RUNOOB 的字符数`SELECT CHAR_LENGTH("RUNOOB") AS LengthOfString;` |
| CHARACTER_LENGTH(s)                   | 返回字符串 s 的字符数，等同于 CHAR_LENGTH(s)                 | 返回字符串 RUNOOB 的字符数`SELECT CHARACTER_LENGTH("RUNOOB") AS LengthOfString;` |
| CONCAT(s1,s2...sn)                    | 字符串 s1,s2 等多个字符串合并为一个字符串                    | 合并多个字符串`SELECT CONCAT("SQL ", "Runoob ", "Gooogle ", "Facebook") AS ConcatenatedString;` |
| CONCAT_WS(x, s1,s2...sn)              | 同 CONCAT(s1,s2,...) 函数，但是每个字符串之间要加上 x，x 可以是分隔符 | 合并多个字符串，并添加分隔符：`SELECT CONCAT_WS("-", "SQL", "Tutorial", "is", "fun!")AS ConcatenatedString;` |
| FIELD(s,s1,s2...)                     | 返回第一个字符串 s 在字符串列表(s1,s2...)中的位置            | 返回字符串 c 在列表值中的位置：`SELECT FIELD("c", "a", "b", "c", "d", "e");` |
| FIND_IN_SET(s1,s2)                    | 返回在字符串s2中与s1匹配的字符串的位置                       | 返回字符串 c 在指定字符串中的位置：`SELECT FIND_IN_SET("c", "a,b,c,d,e");` |
| FORMAT(x,n)                           | 函数可以将数字 x 进行格式化 "#,###.##", 将 x 保留到小数点后 n 位，最后一位四舍五入。 | 格式化数字 "#,###.##" 形式：`SELECT FORMAT(250500.5634, 2);     -- 输出 250,500.56` |
| INSERT(s1,x,len,s2)                   | 字符串 s2 替换 s1 的 x 位置开始长度为 len 的字符串           | 从字符串第一个位置开始的 6 个字符替换为 runoob：`SELECT INSERT("google.com", 1, 6, "runoob");  -- 输出：runoob.com` |
| LOCATE(s1,s)                          | 从字符串 s 中获取 s1 的开始位置                              | 获取 b 在字符串 abc 中的位置：`SELECT LOCATE('st','myteststring');  -- 5`返回字符串 abc 中 b 的位置：`SELECT LOCATE('b', 'abc') -- 2` |
| LCASE(s)                              | 将字符串 s 的所有字母变成小写字母                            | 字符串 RUNOOB 转换为小写：`SELECT LCASE('RUNOOB') -- runoob` |
| LEFT(s,n)                             | 返回字符串 s 的前 n 个字符                                   | 返回字符串 runoob 中的前两个字符：`SELECT LEFT('runoob',2) -- ru` |
| LOWER(s)                              | 将字符串 s 的所有字母变成小写字母                            | 字符串 RUNOOB 转换为小写：`SELECT LOWER('RUNOOB') -- runoob` |
| LPAD(s1,len,s2)                       | 在字符串 s1 的开始处填充字符串 s2，使字符串长度达到 len      | 将字符串 xx 填充到 abc 字符串的开始处：`SELECT LPAD('abc',5,'xx') -- xxabc` |
| LTRIM(s)                              | 去掉字符串 s 开始处的空格                                    | 去掉字符串 RUNOOB开始处的空格：`SELECT LTRIM("    RUNOOB") AS LeftTrimmedString;-- RUNOOB` |
| MID(s,n,len)                          | 从字符串 s 的 n 位置截取长度为 len 的子字符串，同 SUBSTRING(s,n,len) | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT MID("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| POSITION(s1 IN s)                     | 从字符串 s 中获取 s1 的开始位置                              | 返回字符串 abc 中 b 的位置：`SELECT POSITION('b' in 'abc') -- 2` |
| REPEAT(s,n)                           | 将字符串 s 重复 n 次                                         | 将字符串 runoob 重复三次：`SELECT REPEAT('runoob',3) -- runoobrunoobrunoob` |
| REPLACE(s,s1,s2)                      | 将字符串 s2 替代字符串 s 中的字符串 s1                       | 将字符串 abc 中的字符 a 替换为字符 x：`SELECT REPLACE('abc','a','x') --xbc` |
| REVERSE(s)                            | 将字符串s的顺序反过来                                        | 将字符串 abc 的顺序反过来：`SELECT REVERSE('abc') -- cba`    |
| RIGHT(s,n)                            | 返回字符串 s 的后 n 个字符                                   | 返回字符串 runoob 的后两个字符：`SELECT RIGHT('runoob',2) -- ob` |
| RPAD(s1,len,s2)                       | 在字符串 s1 的结尾处添加字符串 s2，使字符串的长度达到 len    | 将字符串 xx 填充到 abc 字符串的结尾处：`SELECT RPAD('abc',5,'xx') -- abcxx` |
| RTRIM(s)                              | 去掉字符串 s 结尾处的空格                                    | 去掉字符串 RUNOOB 的末尾空格：`SELECT RTRIM("RUNOOB     ") AS RightTrimmedString;   -- RUNOOB` |
| SPACE(n)                              | 返回 n 个空格                                                | 返回 10 个空格：`SELECT SPACE(10);`                          |
| STRCMP(s1,s2)                         | 比较字符串 s1 和 s2，如果 s1 与 s2 相等返回 0 ，如果 s1>s2 返回 1，如果 s1<s2 返回 -1 | 比较字符串：`SELECT STRCMP("runoob", "runoob");  -- 0`       |
| SUBSTR(s, start, length)              | 从字符串 s 的 start 位置截取长度为 length 的子字符串         | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT SUBSTR("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| SUBSTRING(s, start, length)           | 从字符串 s 的 start 位置截取长度为 length 的子字符串，等同于 SUBSTR(s, start, length) | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT SUBSTRING("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| SUBSTRING_INDEX(s, delimiter, number) | 返回从字符串 s 的第 number 个出现的分隔符 delimiter 之后的子串。 如果 number 是正数，返回第 number 个字符左边的字符串。 如果 number 是负数，返回第(number 的绝对值(从右边数))个字符右边的字符串。 | `SELECT SUBSTRING_INDEX('a*b','*',1) -- a SELECT SUBSTRING_INDEX('a*b','*',-1)  -- b SELECT SUBSTRING_INDEX(SUBSTRING_INDEX('a*b*c*d*e','*',3),'*',-1)  -- c` |
| TRIM(s)                               | 去掉字符串 s 开始和结尾处的空格                              | 去掉字符串 RUNOOB 的首尾空格：`SELECT TRIM('    RUNOOB    ') AS TrimmedString;` |
| UCASE(s)                              | 将字符串转换为大写                                           | 将字符串 runoob 转换为大写：`SELECT UCASE("runoob"); -- RUNOOB` |
| UPPER(s)                              | 将字符串转换为大写                                           | 将字符串 runoob 转换为大写：`SELECT UPPER("runoob"); -- RUNOOB` |

### 数值函数

| 函数名                             | 描述                                                         | 实例                                                         |
| :--------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| ABS(x)                             | 返回 x 的绝对值                                              | 返回 -1 的绝对值：`SELECT ABS(-1) -- 返回1`                  |
| ACOS(x)                            | 求 x 的反余弦值（单位为弧度），x 为一个数值                  | `SELECT ACOS(0.25);`                                         |
| ASIN(x)                            | 求反正弦值（单位为弧度），x 为一个数值                       | `SELECT ASIN(0.25);`                                         |
| ATAN(x)                            | 求反正切值（单位为弧度），x 为一个数值                       | `SELECT ATAN(2.5);`                                          |
| ATAN2(n, m)                        | 求反正切值（单位为弧度）                                     | `SELECT ATAN2(-0.8, 2);`                                     |
| AVG(expression)                    | 返回一个表达式的平均值，expression 是一个字段                | 返回 Products 表中Price 字段的平均值：`SELECT AVG(Price) AS AveragePrice FROM Products;` |
| CEIL(x)                            | 返回大于或等于 x 的最小整数                                  | `SELECT CEIL(1.5) -- 返回2`                                  |
| CEILING(x)                         | 返回大于或等于 x 的最小整数                                  | `SELECT CEILING(1.5); -- 返回2`                              |
| COS(x)                             | 求余弦值(参数是弧度)                                         | `SELECT COS(2);`                                             |
| COT(x)                             | 求余切值(参数是弧度)                                         | `SELECT COT(6);`                                             |
| COUNT(expression)                  | 返回查询的记录总数，expression 参数是一个字段或者 * 号       | 返回 Products 表中 products 字段总共有多少条记录：`SELECT COUNT(ProductID) AS NumberOfProducts FROM Products;` |
| DEGREES(x)                         | 将弧度转换为角度                                             | `SELECT DEGREES(3.1415926535898) -- 180`                     |
| n DIV m                            | 整除，n 为被除数，m 为除数                                   | 计算 10 除于 5：`SELECT 10 DIV 5;  -- 2`                     |
| EXP(x)                             | 返回 e 的 x 次方                                             | 计算 e 的三次方：`SELECT EXP(3) -- 20.085536923188`          |
| FLOOR(x)                           | 返回小 于或等于 x 的最大整数                                 | 小于或等于 1.5 的整数：`SELECT FLOOR(1.5) -- 返回1`          |
| GREATEST(expr1, expr2, expr3, ...) | 返回列表中的最大值                                           | 返回以下数字列表中的最大值：`SELECT GREATEST(3, 12, 34, 8, 25); -- 34`返回以下字符串列表中的最大值：`SELECT GREATEST("Google", "Runoob", "Apple");   -- Runoob` |
| LEAST(expr1, expr2, expr3, ...)    | 返回列表中的最小值                                           | 返回以下数字列表中的最小值：`SELECT LEAST(3, 12, 34, 8, 25); -- 3`返回以下字符串列表中的最小值：`SELECT LEAST("Google", "Runoob", "Apple");   -- Apple` |
| LN                                 | 返回数字的自然对数，以 e 为底。                              | 返回 2 的自然对数：`SELECT LN(2);  -- 0.6931471805599453`    |
| LOG(x) 或 LOG(base, x)             | 返回自然对数(以 e 为底的对数)，如果带有 base 参数，则 base 为指定带底数。 | `SELECT LOG(20.085536923188) -- 3 SELECT LOG(2, 4); -- 2`    |
| LOG10(x)                           | 返回以 10 为底的对数                                         | `SELECT LOG10(100) -- 2`                                     |
| LOG2(x)                            | 返回以 2 为底的对数                                          | 返回以 2 为底 6 的对数：`SELECT LOG2(6);  -- 2.584962500721156` |
| MAX(expression)                    | 返回字段 expression 中的最大值                               | 返回数据表 Products 中字段 Price 的最大值：`SELECT MAX(Price) AS LargestPrice FROM Products;` |
| MIN(expression)                    | 返回字段 expression 中的最小值                               | 返回数据表 Products 中字段 Price 的最小值：`SELECT MIN(Price) AS MinPrice FROM Products;` |
| MOD(x,y)                           | 返回 x 除以 y 以后的余数                                     | 5 除于 2 的余数：`SELECT MOD(5,2) -- 1`                      |
| PI()                               | 返回圆周率(3.141593）                                        | `SELECT PI() --3.141593`                                     |
| POW(x,y)                           | 返回 x 的 y 次方                                             | 2 的 3 次方：`SELECT POW(2,3) -- 8`                          |
| POWER(x,y)                         | 返回 x 的 y 次方                                             | 2 的 3 次方：`SELECT POWER(2,3) -- 8`                        |
| RADIANS(x)                         | 将角度转换为弧度                                             | 180 度转换为弧度：`SELECT RADIANS(180) -- 3.1415926535898`   |
| RAND()                             | 返回 0 到 1 的随机数                                         | `SELECT RAND() --0.93099315644334`                           |
| ROUND(x [,y])                      | 返回离 x 最近的整数，可选参数 y 表示要四舍五入的小数位数，如果省略，则返回整数。 | `SELECT ROUND(1.23456) --1 SELECT ROUND(345.156, 2) -- 345.16` |
| SIGN(x)                            | 返回 x 的符号，x 是负数、0、正数分别返回 -1、0 和 1          | `SELECT SIGN(-10) -- (-1)`                                   |
| SIN(x)                             | 求正弦值(参数是弧度)                                         | `SELECT SIN(RADIANS(30)) -- 0.5`                             |
| SQRT(x)                            | 返回x的平方根                                                | 25 的平方根：`SELECT SQRT(25) -- 5`                          |
| SUM(expression)                    | 返回指定字段的总和                                           | 计算 OrderDetails 表中字段 Quantity 的总和：`SELECT SUM(Quantity) AS TotalItemsOrdered FROM OrderDetails;` |
| TAN(x)                             | 求正切值(参数是弧度)                                         | `SELECT TAN(1.75);  -- -5.52037992250933`                    |
| TRUNCATE(x,y)                      | 返回数值 x 保留到小数点后 y 位的值（与 ROUND 最大的区别是不会进行四舍五入） | `SELECT TRUNCATE(1.23456,3) -- 1.234`                        |

### 日期函数

| 函数名                                    | 描述                                                         | 实例                                                         |
| :---------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| ADDDATE(d,n)                              | 计算起始日期 d 加上 n 天的日期                               | `SELECT ADDDATE("2017-06-15", INTERVAL 10 DAY); ->2017-06-25` |
| ADDTIME(t,n)                              | n 是一个时间表达式，时间 t 加上时间表达式 n                  | 加 5 秒：`SELECT ADDTIME('2011-11-11 11:11:11', 5); ->2011-11-11 11:11:16 (秒)`添加 2 小时, 10 分钟, 5 秒:`SELECT ADDTIME("2020-06-15 09:34:21", "2:10:5");  -> 2020-06-15 11:44:26` |
| CURDATE()                                 | 返回当前日期                                                 | `SELECT CURDATE(); -> 2018-09-19`                            |
| CURRENT_DATE()                            | 返回当前日期                                                 | `SELECT CURRENT_DATE(); -> 2018-09-19`                       |
| CURRENT_TIME                              | 返回当前时间                                                 | `SELECT CURRENT_TIME(); -> 19:59:02`                         |
| CURRENT_TIMESTAMP()                       | 返回当前日期和时间                                           | `SELECT CURRENT_TIMESTAMP() -> 2018-09-19 20:57:43`          |
| CURTIME()                                 | 返回当前时间                                                 | `SELECT CURTIME(); -> 19:59:02`                              |
| DATE()                                    | 从日期或日期时间表达式中提取日期值                           | `SELECT DATE("2017-06-15");     -> 2017-06-15`               |
| DATEDIFF(d1,d2)                           | 计算日期 d1->d2 之间相隔的天数                               | `SELECT DATEDIFF('2001-01-01','2001-02-02') -> -32`          |
| DATE_ADD(d，INTERVAL expr type)           | 计算起始日期 d 加上一个时间段后的日期，type 值可以是：       | `SELECT DATE_ADD("2017-06-15", INTERVAL 10 DAY);     -> 2017-06-25 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL 15 MINUTE); -> 2017-06-15 09:49:21 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL -3 HOUR); ->2017-06-15 06:34:21 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL -3 MONTH); ->2017-04-15` |
| DATE_FORMAT(d,f)                          | 按表达式 f的要求显示日期 d                                   | `SELECT DATE_FORMAT('2011-11-11 11:11:11','%Y-%m-%d %r') -> 2011-11-11 11:11:11 AM` |
| DATE_SUB(date,INTERVAL expr type)         | 函数从日期减去指定的时间间隔。                               | Orders 表中 OrderDate 字段减去 2 天：`SELECT OrderId,DATE_SUB(OrderDate,INTERVAL 2 DAY) AS OrderPayDate FROM Orders` |
| DAY(d)                                    | 返回日期值 d 的日期部分                                      | `SELECT DAY("2017-06-15");   -> 15`                          |
| DAYNAME(d)                                | 返回日期 d 是星期几，如 Monday,Tuesday                       | `SELECT DAYNAME('2011-11-11 11:11:11') ->Friday`             |
| DAYOFMONTH(d)                             | 计算日期 d 是本月的第几天                                    | `SELECT DAYOFMONTH('2011-11-11 11:11:11') ->11`              |
| DAYOFWEEK(d)                              | 日期 d 今天是星期几，1 星期日，2 星期一，以此类推            | `SELECT DAYOFWEEK('2011-11-11 11:11:11') ->6`                |
| DAYOFYEAR(d)                              | 计算日期 d 是本年的第几天                                    | `SELECT DAYOFYEAR('2011-11-11 11:11:11') ->315`              |
| EXTRACT(type FROM d)                      | 从日期 d 中获取指定的值，type 指定返回的值。 type可取值为：  | `SELECT EXTRACT(MINUTE FROM '2011-11-11 11:11:11')  -> 11`   |
| FROM_DAYS(n)                              | 计算从 0000 年 1 月 1 日开始 n 天后的日期                    | `SELECT FROM_DAYS(1111) -> 0003-01-16`                       |
| HOUR(t)                                   | 返回 t 中的小时值                                            | `SELECT HOUR('1:2:3') -> 1`                                  |
| LAST_DAY(d)                               | 返回给给定日期的那一月份的最后一天                           | `SELECT LAST_DAY("2017-06-20"); -> 2017-06-30`               |
| LOCALTIME()                               | 返回当前日期和时间                                           | `SELECT LOCALTIME() -> 2018-09-19 20:57:43`                  |
| LOCALTIMESTAMP()                          | 返回当前日期和时间                                           | `SELECT LOCALTIMESTAMP() -> 2018-09-19 20:57:43`             |
| MAKEDATE(year, day-of-year)               | 基于给定参数年份 year 和所在年中的天数序号 day-of-year 返回一个日期 | `SELECT MAKEDATE(2017, 3); -> 2017-01-03`                    |
| MAKETIME(hour, minute, second)            | 组合时间，参数分别为小时、分钟、秒                           | `SELECT MAKETIME(11, 35, 4); -> 11:35:04`                    |
| MICROSECOND(date)                         | 返回日期参数所对应的微秒数                                   | `SELECT MICROSECOND("2017-06-20 09:34:00.000023"); -> 23`    |
| MINUTE(t)                                 | 返回 t 中的分钟值                                            | `SELECT MINUTE('1:2:3') -> 2`                                |
| MONTHNAME(d)                              | 返回日期当中的月份名称，如 November                          | `SELECT MONTHNAME('2011-11-11 11:11:11') -> November`        |
| MONTH(d)                                  | 返回日期d中的月份值，1 到 12                                 | `SELECT MONTH('2011-11-11 11:11:11') ->11`                   |
| NOW()                                     | 返回当前日期和时间                                           | `SELECT NOW() -> 2018-09-19 20:57:43`                        |
| PERIOD_ADD(period, number)                | 为 年-月 组合日期添加一个时段                                | `SELECT PERIOD_ADD(201703, 5);    -> 201708`                 |
| PERIOD_DIFF(period1, period2)             | 返回两个时段之间的月份差值                                   | `SELECT PERIOD_DIFF(201710, 201703); -> 7`                   |
| QUARTER(d)                                | 返回日期d是第几季节，返回 1 到 4                             | `SELECT QUARTER('2011-11-11 11:11:11') -> 4`                 |
| SECOND(t)                                 | 返回 t 中的秒钟值                                            | `SELECT SECOND('1:2:3') -> 3`                                |
| SEC_TO_TIME(s)                            | 将以秒为单位的时间 s 转换为时分秒的格式                      | `SELECT SEC_TO_TIME(4320) -> 01:12:00`                       |
| STR_TO_DATE(string, format_mask)          | 将字符串转变为日期                                           | `SELECT STR_TO_DATE("August 10 2017", "%M %d %Y"); -> 2017-08-10` |
| SUBDATE(d,n)                              | 日期 d 减去 n 天后的日期                                     | `SELECT SUBDATE('2011-11-11 11:11:11', 1) ->2011-11-10 11:11:11 (默认是天)` |
| SUBTIME(t,n)                              | 时间 t 减去 n 秒的时间                                       | `SELECT SUBTIME('2011-11-11 11:11:11', 5) ->2011-11-11 11:11:06 (秒)` |
| SYSDATE()                                 | 返回当前日期和时间                                           | `SELECT SYSDATE() -> 2018-09-19 20:57:43`                    |
| TIME(expression)                          | 提取传入表达式的时间部分                                     | `SELECT TIME("19:30:10"); -> 19:30:10`                       |
| TIME_FORMAT(t,f)                          | 按表达式 f 的要求显示时间 t                                  | `SELECT TIME_FORMAT('11:11:11','%r') 11:11:11 AM`            |
| TIME_TO_SEC(t)                            | 将时间 t 转换为秒                                            | `SELECT TIME_TO_SEC('1:12:00') -> 4320`                      |
| TIMEDIFF(time1, time2)                    | 计算时间差值                                                 | `mysql> SELECT TIMEDIFF("13:10:11", "13:10:10"); -> 00:00:01 mysql> SELECT TIMEDIFF('2000:01:01 00:00:00',    ->                 '2000:01:01 00:00:00.000001');        -> '-00:00:00.000001' mysql> SELECT TIMEDIFF('2008-12-31 23:59:59.000001',    ->                 '2008-12-30 01:01:01.000002');        -> '46:58:57.999999'` |
| TIMESTAMP(expression, interval)           | 单个参数时，函数返回日期或日期时间表达式；有2个参数时，将参数加和 | `mysql> SELECT TIMESTAMP("2017-07-23",  "13:10:11"); -> 2017-07-23 13:10:11 mysql> SELECT TIMESTAMP('2003-12-31');        -> '2003-12-31 00:00:00' mysql> SELECT TIMESTAMP('2003-12-31 12:00:00','12:00:00');        -> '2004-01-01 00:00:00'` |
| TIMESTAMPDIFF(unit,datetime_1,datetime_2) | 计算时间差，返回 datetime_expr2 − datetime_expr1 的时间差    | `mysql> SELECT TIMESTAMPDIFF(DAY,'2003-02-01','2003-05-01');   // 计算两个时间相隔多少天        -> 89 mysql> SELECT TIMESTAMPDIFF(MONTH,'2003-02-01','2003-05-01');   // 计算两个时间相隔多少月        -> 3 mysql> SELECT TIMESTAMPDIFF(YEAR,'2002-05-01','2001-01-01');    // 计算两个时间相隔多少年        -> -1 mysql> SELECT TIMESTAMPDIFF(MINUTE,'2003-02-01','2003-05-01 12:05:55');  // 计算两个时间相隔多少分钟        -> 128885` |
| TO_DAYS(d)                                | 计算日期 d 距离 0000 年 1 月 1 日的天数                      | `SELECT TO_DAYS('0001-01-01 01:01:01') -> 366`               |
| WEEK(d)                                   | 计算日期 d 是本年的第几个星期，范围是 0 到 53                | `SELECT WEEK('2011-11-11 11:11:11') -> 45`                   |
| WEEKDAY(d)                                | 日期 d 是星期几，0 表示星期一，1 表示星期二                  | `SELECT WEEKDAY("2017-06-15"); -> 3`                         |
| WEEKOFYEAR(d)                             | 计算日期 d 是本年的第几个星期，范围是 0 到 53                | `SELECT WEEKOFYEAR('2011-11-11 11:11:11') -> 45`             |
| YEAR(d)                                   | 返回年份                                                     | `SELECT YEAR("2017-06-15"); -> 2017`                         |
| YEARWEEK(date, mode)                      | 返回年份及第几周（0到53），mode 中 0 表示周天，1表示周一，以此类推 | `SELECT YEARWEEK("2017-06-15"); -> 201724`                   |

### 高级函数

| 函数名                                                       | 描述                                                         | 实例                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| BIN(x)                                                       | 返回 x 的二进制编码                                          | 15 的 2 进制编码:`SELECT BIN(15); -- 1111`                   |
| BINARY(s)                                                    | 将字符串 s 转换为二进制字符串                                | `SELECT BINARY "RUNOOB"; -> RUNOOB`                          |
| `CASE expression    WHEN condition1 THEN result1    WHEN condition2 THEN result2   ...    WHEN conditionN THEN resultN    ELSE result END` | CASE 表示函数开始，END 表示函数结束。如果 condition1 成立，则返回 result1, 如果 condition2 成立，则返回 result2，当全部不成立则返回 result，而当有一个成立之后，后面的就不执行了。 | `SELECT CASE  　WHEN 1 > 0 　THEN '1 > 0' 　WHEN 2 > 0 　THEN '2 > 0' 　ELSE '3 > 0' 　END ->1 > 0` |
| CAST(x AS type)                                              | 转换数据类型                                                 | 字符串日期转换为日期：`SELECT CAST("2017-08-29" AS DATE); -> 2017-08-29` |
| COALESCE(expr1, expr2, ...., expr_n)                         | 返回参数中的第一个非空表达式（从左向右）                     | `SELECT COALESCE(NULL, NULL, NULL, 'runoob.com', NULL, 'google.com'); -> runoob.com` |
| CONNECTION_ID()                                              | 返回唯一的连接 ID                                            | `SELECT CONNECTION_ID(); -> 4292835`                         |
| CONV(x,f1,f2)                                                | 返回 f1 进制数变成 f2 进制数                                 | `SELECT CONV(15, 10, 2); -> 1111`                            |
| CONVERT(s USING cs)                                          | 函数将字符串 s 的字符集变成 cs                               | `SELECT CHARSET('ABC') ->utf-8     SELECT CHARSET(CONVERT('ABC' USING gbk)) ->gbk` |
| CURRENT_USER()                                               | 返回当前用户                                                 | `SELECT CURRENT_USER(); -> guest@%`                          |
| DATABASE()                                                   | 返回当前数据库名                                             | `SELECT DATABASE();    -> runoob`                            |
| IF(expr,v1,v2)                                               | 如果表达式 expr 成立，返回结果 v1；否则，返回结果 v2。       | `SELECT IF(1 > 0,'正确','错误')     ->正确`                  |
| [IFNULL(v1,v2)](https://www.runoob.com/mysql/mysql-func-ifnull.html) | 如果 v1 的值不为 NULL，则返回 v1，否则返回 v2。              | `SELECT IFNULL(null,'Hello Word') ->Hello Word`              |
| ISNULL(expression)                                           | 判断表达式是否为 NULL                                        | `SELECT ISNULL(NULL); ->1`                                   |
| LAST_INSERT_ID()                                             | 返回最近生成的 AUTO_INCREMENT 值                             | `SELECT LAST_INSERT_ID(); ->6`                               |
| NULLIF(expr1, expr2)                                         | 比较两个字符串，如果字符串 expr1 与 expr2 相等 返回 NULL，否则返回 expr1 | `SELECT NULLIF(25, 25); ->`                                  |
| SESSION_USER()                                               | 返回当前用户                                                 | `SELECT SESSION_USER(); -> guest@%`                          |
| SYSTEM_USER()                                                | 返回当前用户                                                 | `SELECT SYSTEM_USER(); -> guest@%`                           |
| USER()                                                       | 返回当前用户                                                 | `SELECT USER(); -> guest@%`                                  |
| VERSION()                                                    | 返回数据库的版本号                                           | `SELECT VERSION() -> 5.6.34`                                 |

## 常见问题解决

### 数据无法插入中文

> 修改表字符集为utf8

```sql
alter table account convert to character set utf8;
```
