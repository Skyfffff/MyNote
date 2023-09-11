# 信息收集

## 常用端口

## 域名收集

### 用户信息收集

#### WHOIS查询

[*域名Whois查询 - 站长之家 (chinaz.com)](https://whois.chinaz.com/)*

*[whois查询域名查询域名交易_阿里云企航(原万网)-阿里云 (aliyun.com)](https://whois.aliyun.com/)*

*[whois查询-西部数码域名whois查询工具-whois域名查询 (west.cn)](https://whois.west.cn/)*

*[纳网WHOIS查询服务 (nawang.cn)](http://whois.nawang.cn/)*

*[whois查询_域名信息查询-中资源 (zzy.cn)](https://www.zzy.cn/domain/whois.html)*

*[WHOIS SEARCH (35.com)](https://cp.35.com/chinese/whois.php)*

*[新网互联-WHOIS查询 (dns.com.cn)](https://www.dns.com.cn/show/domain/whois/index.do)*

*[美橙域名whois查询-域名whois查询whois查询whois,美橙互联域名whois信息查询中心. (cndns.com)](https://whois.cndns.com/)*

*[域名注册,域名查询,域名申请购买—爱名网 (22.cn)](https://www.22.cn/domain/)*

*[域名 Whois查询,域名注册信息查询,域名网站信息查询 (ename.net)*](https://whois.ename.net/)

#### SEO查询

[SEO综合查询 - 站长工具 (chinaz.com)](https://seo.chinaz.com/)

### 子域名收集

#### 在线收集

[子域名查询 - 站长工具 (chinaz.com)](https://tool.chinaz.com/subdomain/)

[2子域名大全 2二级域名 2域名解析查询 (ip138.com)](https://site.ip138.com/2/domain.htm)

#### 工具收集

**JSFinder：js中收集子域名和url**

https://github.com/Threezh1/JSFinder

**Layer：暴力收集【不推荐】**

[Layer子域名挖掘机4.2纪念版 增加功能 – WebShell'S Blog](https://www.webshell.cc/6384.html)

**oneforall：百家之强推荐**

[shmilylty/OneForAll: OneForAll是一款功能强大的子域收集工具 (github.com)](https://github.com/shmilylty/OneForAll)

### 域名备案查询

[ICP备案查询 - 站长工具 (chinaz.com)](https://icp.chinaz.com/)

[ICP/IP地址/域名信息备案管理系统 (miit.gov.cn)](https://beian.miit.gov.cn/#/Integrated/recordQuery)

### SSL证书查询

[SSL状态检测 (myssl.com)](https://myssl.com/ssl.html)

[SSL证书在线检测工具-中国数字证书CHINASSL](https://www.chinassl.net/ssltools/ssl-checker.html)

## 真实IP收集

### 检查CDN-超级ping

[多个地点Ping服务器,网站测速 - 站长工具 (chinaz.com)](https://ping.chinaz.com/)

### CDN绕过

#### 1.phpinfo

#### 2.奇特ping

##### 泛域名dns

> 如果服务器配置了泛域名dns可以直接ping，有大概率为真实ip。例如ping aaaaaa.jd.com若有响应，大概率为真实ip

##### 海外节点ping

> 如果服务器只设置了中国境内dns，那么海外ping就是其真实的ip地址

#### 3.DNS历史解析记录

> 最早的dns解析记录大概率是真实的ip

[微步在线X情报社区-威胁情报查询_威胁分析平台_开放社区 (threatbook.com)](https://x.threatbook.com/v5/domain/oldqiang.com)

#### 4.子域名绕过

> 收集子域名，有的子域名可能没有上cdn

#### 5.软件绕过

xxx

## 旁站和C段收集

### 概念

**C段:**子网掩码为24的同一网段主机

**旁站：**旁站指的是同一服务器上的其他网站，很多时候，有些网站可能不是那么容易入侵。那么，可以查看该网站所在的服务器上是否还有其他网站。如果有其他网站的话，可以先拿下其他网站的webshell，然后再[提权](https://so.csdn.net/so/search?q=提权&spm=1001.2101.3001.7020)拿到服务器的权限，最后就自然可以拿下该网站了！

### 旁站查询

[同IP网站查询，同服务器网站查询 - 站长工具 (chinaz.com)](https://stool.chinaz.com/same)

### C段扫描

IISPutScanner

## 端口和服务收集

### 利用nmap工具

**详细使用看工具篇！！！**

#### 主机发现

```
-sL: List Scan列表扫描,仅将指定的目标的IP列举出来,不进行主机发现。
-sn: Ping Scan只进行主机发现,不进行端口扫描。
-Pn：将所有指定的主机视作开启的，跳过主机发现的过程。
-PS/PA/PU/PY[portlist]:使用TCPSYN/ACK/SCTP INIT/ECHO方式进行发现。
-PE/PP/PM:使用ICMP echo, timestamp, and netmask 请求包发现主机。
-PO[protocollist]:使用IP协议包探测对方主机是否开启。
-n/-R: -n表示不进行DNS解析；-R表示总是进行DNS解析。
--dns-servers <serv1[,serv2],...>:指定DNS服务器。
--system-dns:指定使用系统的DNS服务器
--traceroute:追踪每个路由节点
```

#### 端口扫描

```
-sS/sT/SA/sw/sM:指定使用TCP SYN/ConnectO/ACK/window/Maimon scans的方式来对目标主机进行扫描。
-SU：指定使用UDP扫描方式确定目标主机的UDP端口状况。
-SN/sF/sX:指定使用TCP Nu11, FIN, and Xmas scans秘密扫描方式来协助探测对方的TCP端口状态。
--scanflags <flags>：定制TCP包的flags.
-sI <zombiehost[:probeport]>:指定使用idle scan方式来扫描目标主机(前提需要找到合适的zombie host)
-SY/sZ： 使用SCTP INIT/COOKIE-ECHO来扫描SCTP协议端口的开放的情况。
-so：使用IP protocol 扫描确定目标机支持的协议类型。
-b <FTP relay host>：使用FTP bounce scan扫描方式

只扫描指定端口
eg: -p22; 
-p1-65535; 
-p U:53,111,137,T:21-25,80,139,8080,S:9
-p <port ranges>
-F 扫描比默认扫描更少的端口
--top-posts <number> 扫描<number>数量的最常见的端口
```

```
open：端口是开放的。
closed：端口是关闭的。
filtered:端口被防火墙IDS/IPS屏蔽,无法确定其状态。
unfiltered：端口没有被屏蔽，但是否开放需要进一步确定。
open|filtered：端口是开放的或被屏蔽。
closed|filtered：端口是关闭的或被屏蔽。
```

```
-T参数,优化时间控制选项的功能很强大也很有效但是有些用户会被迷惑。此外往往选择合适参数的时间超过了所需优化的扫描时间。因此Nmap提供了一些简单的方法使用6个时间模板使用时采用-T选项及数字(0-5)或名称。
模板名称有paranoid(0)、sneaky(1)、polite(2)、normal(3)、aggressive(4)、insane(5)
paranoid、 sneaky模式用于IDS躲避，IDS是入侵检测系统（intrusion detection system，简称"IDS”)是一种对网络传输进行即时监视,在发现可疑传输时发出警报或者采取主动反应措施的网络安全设备
polite模式降低了扫描速度及使用更少的带宽和目标主机资源
normal为默认模式因此-T3实际上未作任何优化
aggressive模式假设用户具有合适及可靠的网络从而加速扫描
insane模式假设用户具有特别快的网络或者愿意为获得速度而牺牲准确性
```

#### 版本侦测

1.首先检查open与openlfiltered状态的端口是否在排除端口列表内。如果在排除列表,将该端口剔除。
2.如果是TCP端口,尝试建立TCP连接。尝试等待片刻(通常6秒或更多,具体时间可以查询文件nmapservices-probes中Probe TCP NULL qll对应的totalwaitms) 。通常在等待时间内,会接收到目标机发送的“welcomeBanner”信息。 nmap将接收到的Banner与nmap-services-probes中NULL probe中的签名进行对比。查找对应应用程序的名字与版本信息。
3.如果通过"welcome Banner"无法确定应用程序版本,那么nmap再尝试发送其他的探测包(即从nmapservices-probes中挑选合适的probe），将probe得到回复包与数据库中的签名进行对比。如果反复探测都无法得出具体应用，那么打印出应用返回报文,让用户自行进一步判定。
4.如果是UDP端口,那么直接使用nmap-services-probes中探测包进行探测匹配。根据结果对比分析出UDP应用服务类型。
5.如果探测到应用程序是SSL,那么调用opensSL进一步的侦查运行在SSL之上的具体的应用类型。
6.如果探测到应用程序是SunRPC，那么调用brute-force RPC grinder进一步探测具体服务。

```
-SV:指定让Nmap进行版本侦测--version-intensity <level>:指定版本侦测强度(0-9) ,默认为7。数值越高,探测出的服务越准确,但是运行时间会比较长。
--version-light：指定使用轻量侦测方式（intensity 2)
--version-all：尝试使用所有的probes进行侦测（intensity 9)
--version-trace：显示出详细的版本侦测过程信息。
```

#### OS侦测

Nmap使用TCP/IP协议栈指纹来识别不同的操作系统和设备。在RFC规范中，有些地方对TCP/IP的实现并没有强制规定,由此不同的TCP/IP方案中可能都有自己的特定方式。Nmap主要是根据这些细节上的差异来判断操作系统的类型的。具体实现方式如下：Nmap内部包含了2600多已知系统的指纹特征（在文件nmap-os-db文件中）。将此指纹数据库作为进行指纹对比的样本库。分别挑选一个open和closed的端口，向其发送经过精心设计的TCP/UDP/ICMP数据包，根据返回的数据包生成一份系统指纹。将探测生成的指纹与nmap-os-db中指纹进行对比，查找匹配的系统。如果无法匹配，以概率形式列举出可能的系统。

```
-O：指定Nmap进行OS侦测。
--osscan-limit: 限制Nmap只对确定的主机的进行os探测（至少需确知该主机分别有一个open和closed的端口）。
--osscan-guess:大胆猜测对方的主机的系统类型。由此准确性会下降不少,但会尽可能多为用户提供潜在的操作系统。
```

#### 漏洞扫描

nmap的漏洞库其实很小，没有多少能扫出来的漏洞，但是它也提供了漏洞扫描功能，我们一般用专业的漏洞扫描工具来扫描。所以简单说一下nmap的漏洞扫描功能，知道即可。

```
扫描端口并且标记可以爆破的服务
nmap 目标（ip地址） --script=auth,vuln 

# 扫描端口并且标记可以爆破的服务
nmap 目标 --script=ftp-brute,imap-brute,smtp-brute,pop3-brute,mongodbbrute,redis-brute,ms-sql-brute,rlogin-brute,rsync-brute,mysql-brute,pgsqlbrute,oracle-sid-brute,oracle-brute, rtsp-url-brute,snmp-brute, svn-brute,telnetbrute,vnc-brute, xmpp-brute

# 精确指定漏洞类型探测并扫描端口
nmap 目标 --script=dns-zone-transfer,ftp-anon,ftp-proftpd-backdoor,ftp-vsftpdbackdoor, ftp-vuln-cve2010-4221,http-backup-finder,http-cisco-anyconnect,http-iisshort-name-brute,http-put,http-php-version,http-shellshock,http-robots.txt,httpsvn-enum,http-webdav-scan,iis-buffer-overflow,iax2-version,memcachedinfo,mongodb-info,msrpc-enum,ms-sql-info,mysql-info,nrpe-enum,pptp-version,redisinfo, rpcinfo, samba-vuln-cve-2012-1182,smb-vuln-ms08-067, smb-vuln-ms17-010, snmpinfo,sshv1,xmpp-info,tftp-enum, teamspeak2-version
```

## 网站指纹信息收集

### 在线收集

[云悉互联网WEB资产在线梳理|在线CMS指纹识别平台 - 云悉安全平台 (yunsee.cn)](https://www.yunsee.cn/)

都不太好用，自查

### 工具收集

wappalyzer浏览器插件

Finger工具

御剑web指纹识别

### 手动收集

xxx

## 敏感信息收集

### 代码托管平台信息泄露

最想强调的是github信息泄露了,直接去github上搜索,收获往往是大于付出。可能有人不自信认为没能力去SRC(安全应急响应中心)挖洞,可是肯定不敢说不会上网不会搜索。github相关的故事太多,但是给人引出的信息泄露远远不仅在这里: github.com, rubygems.org, pan.baidu.com...QQ群备注或介绍等，甚至混入企业qq工作群….. 阿里的oss key都泄露过两次了，就是别人的源代码中泄露出来的。

### 目录信息收集

Dirmap

### Git文件泄露

由于目前的web项目的开发采用前后端完全分离的架构:前端全部使用静态文件,和后端代码完全分离，隶属两个不同的项目。静态文件使用 git 来进行同步发布到服务器，然后使用nginx 指向到指定目录，以达到被公网访问的目的。在运行git init初始化代码库的时候，会在当前目录下面产生一个.git的隐藏文件，用来记录代码的变更记录、日志、目录结构、文件结构等等。在发布代码的时候,没有把.git这个目录删除,就直接发布了。使用这个文件，可以用来恢复源代码

**githack工具**

### Svn文件泄露

Subversion,简称SVN，是一个开放源代码的版本控制系统，相对于的RCS、CVS，采用了分支管理系统,它的设计目标就是取代cvs。互联网上越来越多的控制服务从cvs转移到Subversion, svn出现的比git要早,但是由于svn早期的一些遗留问题,让开发者使用时有些不便,所以git就产生了。其实现在越来越多的公司使用git来进行代码版本的管理。subversion 使用服务端一客户端的结构，当然服务端与客户端可以都运行在同一台服务器上。在服务端是存放着所有受控制数据的subversion仓库,另一端是subversion的客户端程序,管理着受控数据的一部分在本地的映射(称为"工作副本”)。在这两端之间,是通过各种仓库存取层(RepositoryAccess,简称RA)的多条通道进行访问的。这些通道中,可以通过不同的网络协议,例如HTTP、SSH等,或本地文件的方式来对仓库进行操作。

### D_Store文件泄露

.DS_Store 是 Mac 下 Finder 用来保存如何展示文件//文件夹的数据文件，每个文件夹下对应一个。由于开发/设计人员在发布代码时未删除文件夹中隐藏的.DS_store,可能造成文件目录结构泄漏、源代码文件等敏感信息的泄露。我们可以模仿一个环境，利用 phpstudy 搭建 PHP 环境，把 .DS_store 文件上传到相关目录。

工具下载：[lijiejie/ds_store_exp: A .DS_Store file disclosure exploit. It parses .DS_Store file and downloads files recursively. (github.com)](https://github.com/lijiejie/ds_store_exp)

## 搜索引擎信息收集

### Google Hacking

#### 介绍

[Google Hacking 搜索引擎攻击与防范-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1830792)

**Google Hacking**，有时也会被称为 **Google dorking**，是一种利用谷歌搜索的高级使用方式进行信息收集的技术。这个概念最早在2000年由黑客 Johnny Long 提出并推广，一系列关于 Google Hacking 的内容被他写在了《***Google Hacking For Penetration Testers\***》一书中，并受到媒体和大众的关注。在 DEFCON 13的演讲上，Johnny 创造了 “Googledork" 这个词，“Googledork" 指的是“被 Google 透露了信息的愚蠢、无能的人们”。这是为了引起人们注意到，这些信息能被搜索到并不是 Google 的问题，而是由用户或用户安装程序时无意识的错误配置造成的。随着时间的推移，“dork" 这个词成为了“定位敏感信息的搜索”这个行为的简称。

#### 基本用法

 **intitle & allintitle** 

> 使用 intitle 可以搜索网页的的标题，标题指的是在 HTML 中的 title 标签的内容。比如搜索 `intitle:"Index of"` 会返回所有 title 标签中含有关键字短语 “Index of" 的搜索结果。
>
> allintitle 的使用方法和 intitle 类似，但 allintitle 后面可以跟随多个内容。比如 `allintitle:"Index of""backup files"`
>
> 返回所有 title 标签中含有关键字短语 `Index of` 和 `backup files` 的搜索结果。
>
> 但使用 allintitle 会有很大的限制，因为这样搜索的内容只会限制于返回 intitle 的内容，而不能使用别的高级操作符。在实际使用中，最好使用多个 intitle，而不是使用 allintitle。

**allintext** 

> 这个是最容易理解的一个操作符，作用就是返回那些包含搜索内容的页面。当然，allintext 不能与其他高级操作符结合使用。

**inurl & allinurl** 

> 在介绍过 intitle 后，inurl 其实也很好理解：可以搜索网页 url 的内容。然而在实际使用中，inurl 往往并不能如预期般获得想要的结果，原因如下：
>
> Google 并不能很有效地去搜索 url 中协议的部分，比如 `http://`； 在实际情况中，url 通常会包含大量的特殊字符。为了在搜索的同时兼容这些特殊字符，搜索的结果就不会如预期那样精准； 其他的高级操作符（比如：site, filetype 等）可以搜索 url 内特定的部分，在搜索中的效率也比 inurl 高的多。
>
> 所以 inurl 并不如 intitle 那样好用。但即便 inurl 或多或少有一些问题，inurl 在 Google Hacking 中也是不可或缺的。
>
> 和 intitle 相同，inurl 也有一个对应的高级操作符 allinurl。而且 allinurl 同样不能与别的高级操作符结合使用，所以如果想要去搜索 url 中多个关键字，最好使用多个 inurl 操作符。

**site** 

> site 操作符可以在特定的网站中指定搜索内容，比如搜索 `site:apple.com`，返回的内容就只会是 *www.apple.com* 这个域名或者其子域名下的内容。
>
> 不过需要注意的是，**Google “阅读” 域名的顺序是从右到左，和人阅读的顺序是截然相反的**。如果你搜索 `site:aa`，Google 会去搜索以 .aa 为结尾的域名，而不是以 aa 开头的域名。

**filetype** 

> filetype 操作符能搜索的文件类型，也就是指定搜索文件的后缀名。比如搜索 `filetype:php`，搜索将会返回以 php 为结尾的 URL。此操作符往往会与其他高级操作符配合使用，达到更精确的搜索结果。

**link**

> link 操作符可以搜索跳转到指定 URL 的链接，link 操作符后面不仅可以写一些基础 URL，也可以写一些复杂的、完整的 URL。link 操作符也不能与其他高级操作符或关键字一起使用。

 **inanchor** 

> inanchor 操作符可以搜索 HTML 链接标签中的**锚文本**，“锚文本”是网页中关于超链接的一段描述，比如下面这段 HTML 语言：

```js
<a href="http://en.wikipedia.org/wiki/Main_Page">Wikipedia</a>
其中的`Wikipedia`就是这段链接中的锚文本。
```

 **cache** 

> 当 Google 爬到网站的时候，会生成一个链接来保存这个网站的快照，也被称为网页缓存。运用 cache 操作符就可以搜索指定 URL 的网页快照，而且网页快照不会因为原网页的消失或变更而发生改变。

**numrange** 

> numrange 操作符后面需要加上两个数字来表示数字的范围，以 “-" 为分割，形如： `numrange:1234-1235`。当然 Google 也提供了一个更简洁的方式来搜索数字，形如： `1234..1235`，这样就可以不使用 numrange 操作符来达到搜索范围数字的目的了。

 **daterange** 

> daterange 操作符可以搜索指定时间范围内 Google 索引的网站，操作符后面使用的日期格式是“儒略日期（Julian Day）”。关于“儒略日期”的解释请参见相关文档。使用时可以通过在线版查询工具获得需要的“儒略日期"数值，如：*www.onlineconversion.com/julian_date.htm*

 **info** 

> info 操作符会返回一个站点的摘要信息，操作符后面的内容必须是一个完整的站点名称，否则不会返回正确的内容。info 操作符不能与其他操作符一起使用。

**related** 

> related 操作符会搜索那些和输入的 URL 相关或者相似的页面，related 操作符不能与其他操作符一起使用。

**stocks** 

> stocks 操作符会搜索相关的股票信息，stocks 操作符不能与其他操作符一起使用。

 **define** 

> define 操作符会搜索关键字的定义，define 操作符不能与其他操作符一起使用。

### 黑暗搜索引擎

#### Shodan

[Shodan Search Engine](https://www.shodan.io/)

```
hostname:搜索指定的主机或域名,例如hostname:"google"
port:搜索指定的端口或服务，例如 port:"21"
country:搜索指定的国家,例如country:"CN"city:搜索指定的城市,例如city:"Hefei"org:搜索指定的组织或公司,例如org:"google"
isp:搜索指定的ISP供应商,例如isp:"China Telecom"
product:搜索指定的操作系统/软件/平台,例如product:"Apache httpd"
version：搜索指定的软件版本，例如 version:"1.6.2"
geo:搜索指定的地理位置,参数为经纬度,例如geo:"31.8639, 117.2808"
before/after:搜索指定收录时间前后的数据，格式为dd-mm-yy，例如 before:"11-11-15"net:搜索指定的IP地址或子网，例如 net:"210.45.240.0/24"
还可以组合搜索查询中国开发了21端口的网站： country:"CN" port:"21"
```

#### Fofa

[网络空间测绘，网络空间安全搜索引擎，网络空间搜索引擎，安全态势感知 - FOFA网络空间测绘系统](https://fofa.info/)

#### 钟馗之眼

[首页 - 网络空间测绘,网络安全,漏洞分析,动态测绘,钟馗之眼,时空测绘,赛博测绘 - ZoomEye("钟馗之眼")网络空间搜索引擎](https://www.zoomeye.org/)

### 综合工具

ARL灯塔资产侦察系统

## 移动端信息收集

### App抓包收集



### App逆向收集

androidkiller
