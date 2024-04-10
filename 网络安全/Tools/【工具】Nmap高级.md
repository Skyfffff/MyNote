# Nmap基础

## 常用命令

```
sudo nmap -sn 192.168.1.0/24
```

扫描指定的网络地址进行主机发现 -sn只进行主机发现

```
sudo namp -sT --min-rate 10000  -p- 192.168.1.x -oA nampscan/ports
```

对指定的主机进行全端口扫描

-sT 虽然慢一些，但是准确性高

--min-rate 指定最低速率，大佬都用1w，红队测试要更慢一些

```
sudo nmap -sT -sV -sC -O -p21,22,80,3306 192.168.40.133 -oA nmapscan/detail
```

对主机特定端口进行详细扫描

-sV 扫描服务版本

-sC 指定默认脚本

## 主机发现

`-sL`: 列表扫描 - 仅列出要扫描的目标
`-sn`: Ping扫描 - 禁用端口扫描 只进行主机发现,不进行端口扫描

```
老版本是-sP
默认会发送四种请求扫描互联网情况：
第一是ICMP回显请求
第二是对443端口的TCP SYN请求
第三是对对端口80的TCP ACK请求
第四是默认情况下的ICMP时间戳请求
没有sudo权限：
调用connect发送SYN数据包到目标主机的80和443端口
有sudo权限并且扫描本地网络时：
会发送ARP请求，因为ARP是数据链路层的协议，就没有TCP和ICMP的事情
该请求类似sudo arp scan -l
namp也可以加入--send-ip参数这样就可以发送ICMP时间戳请求，因为不一定所有主机都会对arp进行回应，所以ICMP时间戳更可靠
```

`-Pn`: 将所有主机视为在线 - 跳过主机发现
`-PS/PA/PU/PY`[端口列表]: 对给定端口进行TCP SYN/ACK、UDP或SCTP探测
`-PE/PP/PM`: ICMP echo、timestamp和netmask请求探测探测
`-PO`[协议列表]: IP协议Ping
`-n/-R`: 从不进行DNS解析/总是解析 [默认: 有时]
`--dns-servers` <服务器1[,服务器2],...>: 指定自定义DNS服务器
`--system-dns`: 使用操作系统的DNS解析器
`--traceroute`: 跟踪到每个主机的跃点路径

## 端口扫描

```
-sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon扫描
-sU: UDP扫描
-sN/sF/sX: TCP Null、FIN和Xmas扫描
--scanflags <标志>: 自定义TCP扫描标志
-sI <僵尸主机[:探测端口]>: 空闲扫描
-sY/sZ: SCTP INIT/COOKIE-ECHO扫描
-sO: IP协议扫描
-b <FTP中继主机>: FTP反弹扫描
```

### 端口状态

| 状态             | 说明                                     |
| ---------------- | ---------------------------------------- |
| open             | 端口是开放的                             |
| closed           | 端口是关闭的                             |
| filtered         | 端口被防火墙IDS/IPS屏蔽,无法确定其状态   |
| unfiltered       | 端口没有被屏蔽，但是否开放需要进一步确定 |
| open\|filtered   | 端口是开放的或被屏蔽                     |
| closed\|filtered | 端口是关闭的或被屏蔽                     |

### 扫描技术区别

## 版本侦测

```
-sV: 探测打开端口以确定服务/版本信息
--version-intensity <级别>: 设置从0 (轻量级)到9 (尝试所有探测)的级别
--version-light: 限制到最可能的探测 (强度2)
--version-all: 尝试每个探测 (强度9)
--version-trace: 显示详细的版本扫描活动 (用于调试)
```
1.首先检查open与openlfiltered状态的端口是否在排除端口列表内。如果在排除列表,将该端口剔除。
2.如果是TCP端口,尝试建立TCP连接。尝试等待片刻(通常6秒或更多,具体时间可以查询文件nmapservices-probes中Probe TCP NULL qll对应的totalwaitms) 。通常在等待时间内,会接收到目标机发送的“welcomeBanner”信息。 nmap将接收到的Banner与nmap-services-probes中NULL probe中的签名进行对比。查找对应应用程序的名字与版本信息。
3.如果通过"welcome Banner"无法确定应用程序版本,那么nmap再尝试发送其他的探测包(即从nmapservices-probes中挑选合适的probe），将probe得到回复包与数据库中的签名进行对比。如果反复探测都无法得出具体应用，那么打印出应用返回报文,让用户自行进一步判定。
4.如果是UDP端口,那么直接使用nmap-services-probes中探测包进行探测匹配。根据结果对比分析出UDP应用服务类型。
5.如果探测到应用程序是SSL,那么调用opensSL进一步的侦查运行在SSL之上的具体的应用类型。
6.如果探测到应用程序是SunRPC，那么调用brute-force RPC grinder进一步探测具体服务。

## OS侦测

```
-O：指定Nmap进行OS侦测。
--osscan-limit: 限制Nmap只对确定的主机的进行os探测（至少需确知该主机分别有一个open和closed的端口）。
--osscan-guess:大胆猜测对方的主机的系统类型。由此准确性会下降不少,但会尽可能多为用户提供潜在的操作系统。
```
Nmap使用TCP/IP协议栈指纹来识别不同的操作系统和设备。在RFC规范中，有些地方对TCP/IP的实现并没有强制规定,由此不同的TCP/IP方案中可能都有自己的特定方式。Nmap主要是根据这些细节上的差异来判断操作系统的类型的。具体实现方式如下：Nmap内部包含了2600多已知系统的指纹特征（在文件nmap-os-db文件中）。将此指纹数据库作为进行指纹对比的样本库。分别挑选一个open和closed的端口，向其发送经过精心设计的TCP/UDP/ICMP数据包，根据返回的数据包生成一份系统指纹。将探测生成的指纹与nmap-os-db中指纹进行对比，查找匹配的系统。如果无法匹配，以概率形式列举出可能的系统。
## 脚本扫描

nmap的漏洞库其实很小，没有多少能扫出来的漏洞，但是它也提供了漏洞扫描功能，我们一般用专业的漏洞扫描工具来扫描。所以简单说一下nmap的漏洞扫描功能，知道即可。

```
扫描端口并且标记可以爆破的服务
nmap 目标（ip地址） --script=auth,vuln 

# 扫描端口并且标记可以爆破的服务
nmap 目标 --script=ftp-brute,imap-brute,smtp-brute,pop3-brute,mongodbbrute,redis-brute,ms-sql-brute,rlogin-brute,rsync-brute,mysql-brute,pgsqlbrute,oracle-sid-brute,oracle-brute, rtsp-url-brute,snmp-brute, svn-brute,telnetbrute,vnc-brute, xmpp-brute

# 精确指定漏洞类型探测并扫描端口
nmap 目标 --script=dns-zone-transfer,ftp-anon,ftp-proftpd-backdoor,ftp-vsftpdbackdoor, ftp-vuln-cve2010-4221,http-backup-finder,http-cisco-anyconnect,http-iisshort-name-brute,http-put,http-php-version,http-shellshock,http-robots.txt,httpsvn-enum,http-webdav-scan,iis-buffer-overflow,iax2-version,memcachedinfo,mongodb-info,msrpc-enum,ms-sql-info,mysql-info,nrpe-enum,pptp-version,redisinfo, rpcinfo, samba-vuln-cve-2012-1182,smb-vuln-ms08-067, smb-vuln-ms17-010, snmpinfo,sshv1,xmpp-info,tftp-enum, teamspeak2-version
```

```
auth: 负责处理鉴权证书（绕开鉴权）的脚本  
broadcast: 在局域网内探查更多服务开启状况，如dhcp/dns/sqlserver等服务  
brute: 提供暴力破解方式，针对常见的应用如http/snmp等  
default: 使用-sC或-A选项扫描时候默认的脚本，提供基本脚本扫描能力  
discovery: 对网络进行更多的信息，如SMB枚举、SNMP查询等  
dos: 用于进行拒绝服务攻击  
exploit: 利用已知的漏洞入侵系统  
external: 利用第三方的数据库或资源，例如进行whois解析  
fuzzer: 模糊测试的脚本，发送异常的包到目标机，探测出潜在漏洞 intrusive: 入侵性的脚本，此类脚本可能引发对方的IDS/IPS的记录或屏蔽  
malware: 探测目标机是否感染了病毒、开启了后门等信息  
safe: 此类与intrusive相反，属于安全性脚本  
version: 负责增强服务与版本扫描（Version Detection）功能的脚本  
vuln: 负责检查目标机是否有常见的漏洞（Vulnerability），如是否有MS08_067
```

有待完善。。。

## nmap命令译

```
Nmap 7.94 ( https://nmap.org )
用法: nmap [扫描类型] [选项] {目标规范}
目标规范:
  可以传递主机名、IP地址、网络等。
  例如：scanme.nmap.org、microsoft.com/24、192.168.0.1；10.0.0-255.1-254
  -iL <输入文件名>: 从主机/网络列表中读取
  -iR <主机数>: 随机选择目标
  --exclude <主机1[,主机2][,主机3],...>: 排除主机/网络
  --excludefile <排除文件>: 从文件中排除列表
主机发现:
  -sL: 列表扫描 - 仅列出要扫描的目标
  -sn: Ping扫描 - 禁用端口扫描 只进行主机发现,不进行端口扫描
  -Pn: 将所有主机视为在线 - 跳过主机发现
  -PS/PA/PU/PY[端口列表]: 对给定端口进行TCP SYN/ACK、UDP或SCTP探测
  -PE/PP/PM: ICMP echo、timestamp和netmask请求探测探测
  -PO[协议列表]: IP协议Ping
  -n/-R: 从不进行DNS解析/总是解析 [默认: 有时]
  --dns-servers <服务器1[,服务器2],...>: 指定自定义DNS服务器
  --system-dns: 使用操作系统的DNS解析器
  --traceroute: 跟踪到每个主机的跃点路径
扫描技术:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon扫描
  -sU: UDP扫描
  -sN/sF/sX: TCP Null、FIN和Xmas扫描
  --scanflags <标志>: 自定义TCP扫描标志
  -sI <僵尸主机[:探测端口]>: 空闲扫描
  -sY/sZ: SCTP INIT/COOKIE-ECHO扫描
  -sO: IP协议扫描
  -b <FTP中继主机>: FTP反弹扫描
端口规范和扫描顺序:
  -p <端口范围>: 仅扫描指定端口
    例如: -p22；-p1-65535；-p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <端口范围>: 从扫描中排除指定的端口
  -F: 快速模式 - 扫描比默认扫描更少的端口
  -r: 顺序扫描端口 - 不随机化
  --top-ports <数量>: 扫描<数量>个最常见的端口
  --port-ratio <比例>: 扫描比<比例>更常见的端口
服务/版本检测:
  -sV: 探测打开端口以确定服务/版本信息
  --version-intensity <级别>: 设置从0 (轻量级)到9 (尝试所有探测)的级别
  --version-light: 限制到最可能的探测 (强度2)
  --version-all: 尝试每个探测 (强度9)
  --version-trace: 显示详细的版本扫描活动 (用于调试)
脚本扫描:
  -sC: 等效于 --script=default
  --script=<Lua脚本>: <Lua脚本>是一个逗号分隔的目录、脚本文件或脚本类别列表
  --script-args=<n1=v1,[n2=v2,...]>: 为脚本提供参数
  --script-args-file=文件名: 在文件中提供NSE脚本参数
  --script-trace: 显示发送和接收的所有数据
  --script-updatedb: 更新脚本数据库。
  --script-help=<Lua脚本>: 显示关于脚本的帮助。
           <Lua脚本>是一个逗号分隔的脚本文件或脚本类别列表。
操作系统检测:
  -O: 启用操作系统检测
  --osscan-limit: 限制OS检测到有希望的目标
  --osscan-guess: 更积极地猜测操作系统
时间和性能:
  带有<时间>的选项以秒为单位，或附加'ms' (毫秒)、's' (秒)、'm' (分钟)或'h' (小时)的值 (例如：30m)。
  -T<0-5>: 设置时间模板 (更高表示更快)
  --min-hostgroup/max-hostgroup <大小>: 并行主机扫描组大小
  --min-parallelism/max-parallelism <numprobes>: 探测并行化
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <时间>: 指定探测的往返时间。
  --max-retries <尝试次数>: 限制端口扫描探测的重传次数。
  --host-timeout <时间>: 在此时间后放弃目标
  --scan-delay/--max-scan-delay <时间>: 调整探测之间的延迟
  --min-rate <数字>: 每秒发送的数据包不慢于<数字>
  --max-rate <数字>: 每秒发送的数据包不快于<数字>
防火墙/IDS规避和欺骗:
  -f; --mtu <值>: 分段数据包 (可选附带MTU值)
  -D <假目标1,假目标2[,ME],...>: 用伪装的

扫描来掩盖真实扫描
  -S <IP地址>: 伪装源地址
  -e <接口>: 使用指定的接口
  -g/--source-port <端口号>: 使用给定的端口号
  --proxies <url1,[url2],...>: 通过HTTP/SOCKS4代理中继连接
  --data <十六进制字符串>: 将自定义有效负载附加到发送的数据包
  --data-string <字符串>: 将自定义ASCII字符串附加到发送的数据包
  --data-length <数字>: 将随机数据附加到发送的数据包
  --ip-options <选项>: 使用指定的IP选项发送数据包
  --ttl <值>: 设置IP生存时间字段
  --spoof-mac <MAC地址/前缀/供应商名称>: 伪装MAC地址
  --badsum: 发送具有伪造的TCP/UDP/SCTP校验和的数据包
输出:
  -oN/-oX/-oS/-oG <文件>: 将扫描输出以普通、XML、s|<rIpt kIddi3和Grepable格式输出到指定文件。
  -oA <基本名称>: 同时以三种主要格式输出
  -v: 增加详细级别 (使用-vv或更高以获得更大的效果)
  -d: 增加调试级别 (使用-dd或更高以获得更大的效果)
  --reason: 显示端口处于特定状态的原因
  --open: 仅显示打开的 (或可能打开的)端口
  --packet-trace: 显示发送和接收的所有数据包
  --iflist: 打印主机接口和路由 (用于调试)
  --append-output: 追加而不是覆盖指定的输出文件
  --resume <文件名>: 恢复中断的扫描
  --noninteractive: 禁用通过键盘的运行时交互
  --stylesheet <路径/URL>: 用于将XML输出转换为HTML的XSL样式表
  --webxml: 引用Nmap.Org的样式表，以获得更便携的XML
  --no-stylesheet: 防止将XSL样式表与XML输出关联
其他:
  -6: 启用IPv6扫描
  -A: 启用操作系统检测、版本检测、脚本扫描和跟踪
  --datadir <目录名>: 指定自定义Nmap数据文件位置
  --send-eth/--send-ip: 使用原始以太网帧或IP数据包发送
  --privileged: 假设用户具有完全特权
  --unprivileged: 假设用户缺少原始套接字特权
  -V: 打印版本号
  -h: 打印此帮助摘要页面。
示例:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
有关更多选项和示例，请参阅MAN页面 (https://nmap.org/book/man.html)。
```

