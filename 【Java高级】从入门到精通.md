# Java高级&底层源码

## Socket网络编程

### UDP网络编程

#### UDP协议

- 概念

  > xxxx

- 特点

  - 面向无连接

  - 不可靠协议

  - 安全系数低
  - 容易丢包
  - 传输速度快

- 应用
  - 聊天工具

#### 获取ip

> 可通过hosts修改dns解析

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class socketTest01 {
    public static void main(String[] args) throws UnknownHostException {
        //获取给点主机名的ip地址
        InetAddress inetAddress = InetAddress.getByName("www.baidu.com");
        System.out.println(inetAddress);
        //获取本地主机ip
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println(localHost);
    }
}
```

#### UDP发送端 

```java
import java.io.IOException;
import java.net.*;

public class udpClient  {
    public static void main(String[] args) throws IOException {
        //创建发送端socket对象
        DatagramSocket socket = new DatagramSocket();
        //封装数据
        byte[] data = "helloworld你好123456".getBytes();
        //获取要发送的地址
        InetAddress host = InetAddress.getByName("sky");
        //封装
        DatagramPacket packet = new DatagramPacket(data,data.length,host,8080);
        //发送数据
        socket.send(packet);
        //释放资源
        socket.close();
    }
}
```

#### UDP接收端

```java
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class updServer {
    public static void main(String[] args) throws IOException {
        //开始监听端口
        DatagramSocket socket = new DatagramSocket(8080);
        //创建缓存区
        DatagramPacket packet = new DatagramPacket(new byte[1024],1024);
        //接收数据，开始阻塞
        socket.receive(packet);
        //获取数据
        byte[] data = packet.getData();
        //解析数据
        String msg = new String(data,0,packet.getLength());
        //打印数据
        System.out.println(msg);
        //释放资源
        socket.close();
    }
}
```

#### UDP发送端改进

> 连续发送

```java
import java.io.IOException;
import java.net.*;
import java.util.Scanner;

public class udpClientTest01 {
    public static void main(String[] args) throws IOException {
        while (true) {
            //创建发送端socket对象
            DatagramSocket socket = new DatagramSocket();
            //封装数据
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入：");
            String context = sc.nextLine();
            byte[] data = context.getBytes();
            //获取要发送的地址
            InetAddress host = InetAddress.getByName("192.168.0.104");
            //封装
            DatagramPacket packet = new DatagramPacket(data,data.length,host,9999);
            //发送数据
            socket.send(packet);
            socket.close();
        }
    }
}

```

#### UDP接收端改进

> 持续接收

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class updServerTest01 {
    public static void main(String[] args) throws IOException {
        //开始监听端口
        DatagramSocket socket = new DatagramSocket(9999);
        //创建缓存区
        DatagramPacket packet = new DatagramPacket(new byte[1024],1024);
        while (true){
            //接收数据，开始阻塞
            socket.receive(packet);
            //获取数据
            byte[] data = packet.getData();
            //解析数据
            String msg = new String(data, 0, packet.getLength());
            //打印数据
            System.out.println(packet.getSocketAddress());
            System.out.println(msg);
        }
    }
}
```

### TCP网络编程

#### TCP协议

xxx

#### TCP发送端

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class tcpClientTest01 {
    public static void main(String[] args) throws IOException {
        //创建socket链接对象
        Socket socket = new Socket(InetAddress.getLocalHost(),9999);
        //获取输出流
        OutputStream ops = socket.getOutputStream();
        //发送数据
        ops.write("Sky最帅".getBytes());
        //释放资源
        ops.close();
        socket.close();
    }
}
```

#### TCP接收端

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class tcpServerTest01 {
    public static void main(String[] args) throws IOException {
        //监听端口
        ServerSocket socket = new ServerSocket(9999);
        //接收数据
        Socket accept = socket.accept();
        InputStream is = accept.getInputStream();
        //解析数据
        byte[] bytes = new byte[1024];
        int length = is.read(bytes);
        System.out.println(new String(bytes,0,length));
        //释放资源
        socket.close();
        is.close();
    }
}
```

#### TCP发送端改进

> 连续发送

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

public class tcpClientTest01 {
    public static void main(String[] args) throws IOException {
        while (true) {
            //创建socket链接对象
            Socket socket = new Socket("sky", 5656);
            //获取输出流
            OutputStream ops = socket.getOutputStream();
            //发送数据
            System.out.println("请输入发送内容：");
            Scanner sc = new Scanner(System.in);
            String msg = sc.nextLine();
            if (sc.equals("close")) {
                break;
            }
            ops.write(msg.getBytes());
            //释放资源
            ops.close();
            socket.close();
        }
    }
}
```

#### TCP接收端改进

> 多线程＋持续接收

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class tcpServerTest01 {
    public static void main(String[] args) throws IOException, InterruptedException {
        ServerSocket sSocket = new ServerSocket(5656);
        while (true) {
            Socket socket = sSocket.accept();
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        InputStream is = socket.getInputStream();
                        byte[] bytes = new byte[1024];
                        int len = is.read(bytes);
                        System.out.println("来自：" + socket.getRemoteSocketAddress());
                        System.out.println(new String(bytes, 0, len));
                        is.close();
                        socket.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }
    }
}
```

### 手写Http

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class httpTcpServer {
    public static void main(String[] args) throws IOException {
        //监听端口
        ServerSocket sSocket = new ServerSocket(80);
        while (true) {
            //拦截请求
            Socket accept = sSocket.accept();
            //从本地读取静态页面到内存
            FileInputStream fis = new FileInputStream(new File("D:\\Sourcecode\\Web前端\\index.html"));
            byte[] bytes = new byte[1024];
            int len = fis.read(bytes);
            //发送数据
            OutputStream ops = accept.getOutputStream();
            ops.write(bytes, 0, len);
            //释放资源
            ops.close();
            accept.close();
        }
    }
}
```

## 多线程

### 实现方法

#### 继承Thread类重写run方法

```java
public class ThreadTest01 extends Thread{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }

    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        //开启子线程
        new ThreadTest01().start();
        new ThreadTest01().start();
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }
}
```

#### 实现Runnable接口

> 实现Runnable接口

```java
public class ThreadTest02 implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }

    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        new Thread(new ThreadTest02()).start();
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }
}
```

> Jdk8匿名内部类

```java
public class ThreadTest02{
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        new Thread(new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"【线程已开启】");
                System.out.println(Thread.currentThread().getName()+"【线程已结束】");
            }
        }).start();
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }
}
```

> lambda表达式

```java
public class ThreadTest02{
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        new Thread(() -> {
            System.out.println(Thread.currentThread().getName()+"【线程已开启】");
            System.out.println(Thread.currentThread().getName()+"【线程已结束】");
        }).start();
        System.out.println(Thread.currentThread().getName()+"【线程已结束】");
    }
}
```
