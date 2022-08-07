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

> UDP发送端改进

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

> UDP接收端改进

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

> TCP发送端改进

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

> TCP接收端改进

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

#### 实现Callable接口

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class ThreadTest03 implements Callable<String> {
    @Override
    public String call() throws Exception {
        System.out.println(Thread.currentThread().getName()+"【线程已开启】");
        return "flag:ture";
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ThreadTest03 threadTest03 = new ThreadTest03();
        FutureTask<String> futureTask = new FutureTask<>(threadTest03);
        //开启线程
        new Thread(futureTask).start();
        //挂起线程。等待返回结果
        String result = futureTask.get();
        System.out.println(Thread.currentThread().getName()+"【"+result+"】");
    }
}
```

#### 线程池

> 匿名内部类

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadTest04 {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newCachedThreadPool();
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"线程开始");
            }
        });
    }
}
```

> lambda表达式

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadTest04 {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newCachedThreadPool();
        executorService.execute(() -> System.out.println(Thread.currentThread().getName()+"线程开始"));
    }
}
```

#### Spring注解@Async

> xxx

### 锁

#### synchronized锁

```java
public class lockTest01 implements Runnable {
    private static int count = 100;

    @Override
    public void run() {
        while (true) {
            if (count < 0) {
                return;
            }
            try {
                Thread.sleep(30);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //上锁(对象锁)
            synchronized (this) {
                count--;
                System.out.println(Thread.currentThread().getName() + "当前值:" + count);
            }
        }
    }

    public static void main(String[] args) {
        lockTest01 lock = new lockTest01();
        new Thread(lock).start();
        new Thread(lock).start();
    }
}
```

#### SpringMvc中上锁注意事项

> **bean是单例的，需要@Scope（value="prototype")注解改成多例**

#### wait和notify

> wait：线程进入WAITING状态，等待其他线程的通知或者被中断，才会返回。使用wait会释放当前锁
>
> notify：通知一个在对象上等待的线程，使其从main方法返回，返回的前提是该线程获取了对象的锁
>
> notifyAll：通知所有等待在该对象的线程

```java
public class lockTest02 {
    private Object object = new Object();

    public static void main(String[] args) throws InterruptedException {
        new lockTest02().cal();
    }

    public void cal() throws InterruptedException {

        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("[1]");
                synchronized (object) {
                    try {
                        //进入WAITING状态，释放锁，阻塞线程
                        object.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println("[2]");
            }
        }).start();

        Thread.sleep(1000);
        synchronized (object) {
            //通知object对象上等待的线程使其从main方法返回，返回的前提是该线程获取了对象的锁
            object.notify();
        }
    }
}
```

### 反射

#### 创建对象

> 调用无参构造器

```java
import domain.User;

public class reflectionTest01 {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
        //反射得到class
        Class<?> user = Class.forName("domain.User");
        //创建无参对象
        User u =(User) user.newInstance();
        System.out.println(u);
    }
}
```

> 调用有参构造器

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class reflectionTest02 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        //反射得到class
        Class<?> user = Class.forName("domain.User");
        //得到构造器
        Constructor<?> constructor = user.getConstructor(String.class, String.class);
        //创建有参对象
        User u = (User) constructor.newInstance("Sky", "root");
        System.out.println(u);
    }
}
```

> 调用私有构造器

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class reflectionTest02 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        //反射得到class
        Class<?> userClass = Class.forName("domain.User");
        //得到构造器（私有公有都可以）
        Constructor<?> constructor = userClass.getDeclaredConstructor(String.class,String.class);
        //暴力开启访问权限
        constructor.setAccessible(true);
        //创建有参对象
        User u = (User) constructor.newInstance("Sky","666");
        System.out.println(u);
    }
}
```

#### 访问属性

```java
import java.lang.reflect.Field;

public class reflectionTest03 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, InstantiationException, IllegalAccessException {
        //反射得到class
        Class<?> userClass = Class.forName("domain.User");
        //反射得到私有属性
        Field userName = userClass.getDeclaredField("userName");
        //暴力开启访问权限
        userName.setAccessible(true);
        //创建对象
        User user = (User) userClass.newInstance();
        //设置属性值
        userName.set(user,"Sky");
        System.out.println(user);
    }
}
```

#### 调用方法

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class reflectionTest04 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        //反射得到class
        Class<?> userClass = Class.forName("domain.User");
        //得到对应方法
        Method getUserName = userClass.getDeclaredMethod("getUserName");
        //开启方法访问权限
        getUserName.setAccessible(true);
        //获取有参构造器
        Constructor<?> constructor = userClass.getConstructor(String.class, String.class);
        //创建对象
        User user = (User) constructor.newInstance("Sky", "root");
        //调用方法
        Object invoke = getUserName.invoke(user);
        System.out.println(invoke);
    }
}
```
