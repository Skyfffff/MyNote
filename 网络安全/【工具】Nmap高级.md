# Nmap高级

## 主机发现

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

## 端口扫描

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

## 版本侦测

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

## OS侦测

Nmap使用TCP/IP协议栈指纹来识别不同的操作系统和设备。在RFC规范中，有些地方对TCP/IP的实现并没有强制规定,由此不同的TCP/IP方案中可能都有自己的特定方式。Nmap主要是根据这些细节上的差异来判断操作系统的类型的。具体实现方式如下：Nmap内部包含了2600多已知系统的指纹特征（在文件nmap-os-db文件中）。将此指纹数据库作为进行指纹对比的样本库。分别挑选一个open和closed的端口，向其发送经过精心设计的TCP/UDP/ICMP数据包，根据返回的数据包生成一份系统指纹。将探测生成的指纹与nmap-os-db中指纹进行对比，查找匹配的系统。如果无法匹配，以概率形式列举出可能的系统。

```
-O：指定Nmap进行OS侦测。
--osscan-limit: 限制Nmap只对确定的主机的进行os探测（至少需确知该主机分别有一个open和closed的端口）。
--osscan-guess:大胆猜测对方的主机的系统类型。由此准确性会下降不少,但会尽可能多为用户提供潜在的操作系统。
```

## 漏洞扫描

nmap的漏洞库其实很小，没有多少能扫出来的漏洞，但是它也提供了漏洞扫描功能，我们一般用专业的漏洞扫描工具来扫描。所以简单说一下nmap的漏洞扫描功能，知道即可。

```
扫描端口并且标记可以爆破的服务
nmap 目标（ip地址） --script=auth,vuln 

# 扫描端口并且标记可以爆破的服务
nmap 目标 --script=ftp-brute,imap-brute,smtp-brute,pop3-brute,mongodbbrute,redis-brute,ms-sql-brute,rlogin-brute,rsync-brute,mysql-brute,pgsqlbrute,oracle-sid-brute,oracle-brute, rtsp-url-brute,snmp-brute, svn-brute,telnetbrute,vnc-brute, xmpp-brute

# 精确指定漏洞类型探测并扫描端口
nmap 目标 --script=dns-zone-transfer,ftp-anon,ftp-proftpd-backdoor,ftp-vsftpdbackdoor, ftp-vuln-cve2010-4221,http-backup-finder,http-cisco-anyconnect,http-iisshort-name-brute,http-put,http-php-version,http-shellshock,http-robots.txt,httpsvn-enum,http-webdav-scan,iis-buffer-overflow,iax2-version,memcachedinfo,mongodb-info,msrpc-enum,ms-sql-info,mysql-info,nrpe-enum,pptp-version,redisinfo, rpcinfo, samba-vuln-cve-2012-1182,smb-vuln-ms08-067, smb-vuln-ms17-010, snmpinfo,sshv1,xmpp-info,tftp-enum, teamspeak2-version
```