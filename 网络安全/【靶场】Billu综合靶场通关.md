# 综合靶场

## 信息收集

### `nmap`

主机：192.168.229.128

```
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 fa:cf:a2:52:c4:fa:f5:75:a7:e2:bd:60:83:3e:7b:de (DSA)
|   2048 88:31:0c:78:98:80:ef:33:fa:26:22:ed:d0:9b:ba:f8 (RSA)
|_  256 0e:5e:33:03:50:c9:1e:b3:e7:51:39:a4:4a:10:64:ca (ECDSA)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-title: --==[[IndiShell Lab]]==--
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.2.22 (Ubuntu)
MAC Address: 00:0C:29:2F:48:54 (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### 御剑

```
http://192.168.229.128/images/
http://192.168.229.128/index.php
http://192.168.229.128/test.php
http://192.168.229.128/add.php
http://192.168.229.128/head.php
http://192.168.229.128/show.php
http://192.168.229.128/c.php
http://192.168.229.128/in.php
```

### dirb

```
---- Scanning URL: http://192.168.229.128/ ----
+ http://192.168.229.128/add (CODE:200|SIZE:307)                                                            
+ http://192.168.229.128/c (CODE:200|SIZE:1)                                                           
+ http://192.168.229.128/cgi-bin/ (CODE:403|SIZE:291)                                                       
+ http://192.168.229.128/head (CODE:200|SIZE:2793)                                                           
==> DIRECTORY: http://192.168.229.128/images/                                                               
+ http://192.168.229.128/in (CODE:200|SIZE:47559) 
+ http://192.168.229.128/index (CODE:200|SIZE:3267)                       
+ http://192.168.229.128/index.php (CODE:200|SIZE:3267)                        
+ http://192.168.229.128/panel (CODE:302|SIZE:2469)                         
+ http://192.168.229.128/server-status (CODE:403|SIZE:296)                    
+ http://192.168.229.128/show (CODE:200|SIZE:1)        
+ http://192.168.229.128/test (CODE:200|SIZE:72)
```

### dirmap

```
[200][text/html][307.00b] http://192.168.229.128/add
[200][text/html][307.00b] http://192.168.229.128/add.php
[200][text/html][1.00b] http://192.168.229.128/c
[200][text/html][46.68kb] http://192.168.229.128/in
[200][text/html][3.19kb] http://192.168.229.128/index
[200][text/html][3.19kb] http://192.168.229.128/index.php
[200][text/html; charset=utf-8][8.17kb] http://192.168.229.128/phpmy/
[200][text/html][1.00b] http://192.168.229.128/show
[200][text/html][72.00b] http://192.168.229.128/test
[200][text/html][72.00b] http://192.168.229.128/test.php
```

### dirsearch

```
# Dirsearch started Wed Nov 15 21:45:59 2023 as: /usr/lib/python3/dist-packages/dirsearch/dirsearch.py -u http://192.168.229.128 -e php,html,js -o output.txt --format=plain

200   307B   http://192.168.229.128/add
200   307B   http://192.168.229.128/add.php
200     1B   http://192.168.229.128/c
200     3KB  http://192.168.229.128/head
200     3KB  http://192.168.229.128/head.php
200   501B   http://192.168.229.128/images/
200    47KB  http://192.168.229.128/in
200     8KB  http://192.168.229.128/phpmy/
200     1B   http://192.168.229.128/show
200    72B   http://192.168.229.128/test
200    72B   http://192.168.229.128/test.php
```

## 信息汇总

主机：192.168.229.128

操作系统：

```
Linux indishell 3.13.0-32-generic #57~precise1-Ubuntu SMP Tue Jul 15 03:50:54 UTC 2014 i686 

Ubuntu 12.04 support ended on 2017-04-30.
Upgrade to Ubuntu 21.04 / LTS 20.04 / LTS 18.04.

Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
```

服务器信息：

```
Apache/2.2.22 (Ubuntu)
```

端口信息：

```
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
```

## 系统提权