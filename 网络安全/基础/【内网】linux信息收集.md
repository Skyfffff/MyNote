## 一、基础信息收集

#### 1.查看系统类型

- cat /etc/issue
- cat /etc/*-release
- cat /etc/lsb-release
- cat /etc/redhat-release

#### 2.内核版本

- cat /proc version
- uname -a
- uname -mrs
- rpm -q kernel
- dmesg |grep Linux
- ls /root |grep vmlinuz

#### 3.进程与服务

- ps aux
- ps -ef
- top
- cat /etc/service

#### 4.安装的应用程序

- dpkg -l
- rpm -qa

#### 5.服务配置

- cat /etc/syslog.conf
- cat /etc/chttp.conf
- cat /etc/lighttpd.cnf
- cat /etc/cups/cupsd.conf
- cat /etc/inetd.conf
- cat /etc/apache2/apache2.conf
- cat /etc/my.conf
- cat /etc/httpd/conf/httpd.conf
- cat /opt/lampp/etc/httpd.conf

#### 6.计划任务

- crontab -l
- ls -alh /var/spool/cron
- ls -al /etc/ | grep cron
- ls -al /etc/cron*
- cat /etc/cron*
- cat /etc/at.allow
- cat /etc/at.deny
- cat /etc/cron.allow
- cat /etc/cron.deny
- cat /etc/crontab
- cat /etc/anacrontab
- cat /var/spool/cron/crontabs/root

#### 7.网络配置

- cat /etc/network/interfaces
- cat /etc/sysconfig/network
- cat /etc/resolv.conf
- cat /etc/sysconfig/network
- cat /etc/networks
- iptables -L
- hostname
- dnsdomainname

#### 8.网络通信

- netstat -antup
- netstat -antpx
- netstat -tulpn
- arp -e
- route

#### 9.用户信息

- id
- who
- w
- last
- cat /etc/sudoers
- cat /etc/passwd
- cat /etc/group
- cat /etc/shadow
- history
- cat ~/.bash_history
- cat ~/.nano_history
- cat ~/.atftp_history
- cat ~/.mysql_history
- cat ~/.php_history
- 设置不良记录 unset HISFILE (键盘记录)

#### 10.日志收集

- /etc/httpd/logs/*
- /var/log/*
- /var/run/utmp

#### 11.可能用于提权的程序

- 查找有suid位或sgid位的程序
  - find / -perm -g =s -o -perm -u=s -type f 2>/dev/null
- 查找能写或进入的目录
  - find / -writable -type d 2 > /dev/null
  - find / -perm -o+w -type d 2 > /dev/null
  - find / -perm -o+x -type d 2 > /dev/null