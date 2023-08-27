# Java高级&底层源码

## 🔸Socket网络编程

### 🔹UDP网络编程

#### ◼UDP协议

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

#### ◼获取ip

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

#### ◼UDP发送端 

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

#### ◼UDP接收端

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

> UDP发送端改进、连续发送

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

> UDP接收端改进、持续接收

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

### 🔹TCP网络编程

#### ◼TCP协议

xxx

#### ◼TCP发送端

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

#### ◼TCP接收端

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

> TCP发送端改进、连续发送

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

> TCP接收端改进、多线程＋持续接收

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

### 🔹手写Http

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

## 🔸多线程

### 🔹实现方法

#### ◼继承Thread类重写run方法

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

#### ◼实现Runnable接口

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

#### ◼实现Callable接口

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

#### ◼线程池

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

#### ◼Spring注解@Async

> xxx

### 🔹锁

#### ◼synchronized锁

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

#### ◼SpringMvc中上锁注意事项

> **bean是单例的，需要@Scope（value="prototype")注解改成多例**

#### ◼wait和notify

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

### 🔸反射

#### 🔹创建对象

- 调用无参构造器

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

- 调用有参构造器

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

- 调用私有构造器

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

#### 🔹访问属性

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

#### 🔹调用方法

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

# JDBC

## 🔸数据库链接

### 🔹方法①

```java
public static void main(String[] args) throws SQLException {
        //创建驱动对象
        Driver driver = new Driver();
        //数据库链接地址
        String url = "jdbc:mysql://localhost:3306/db1";
        //
        Properties properties = new Properties();
        properties.setProperty("user","root");
        properties.setProperty("password","123456");
        //链接数据库
        Connection connect = driver.connect(url, properties);

        System.out.println(connect);

    }
```

### 🔹方法②

```java
 public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, SQLException {
        //使用反射获取Driver实现类
        Class aClass = Class.forName("com.mysql.jdbc.Driver");
        Driver driver = (Driver) aClass.newInstance();

        Properties properties = new Properties();
        properties.setProperty("user", "root");
        properties.setProperty("password", "123456");
        //数据库链接地址
        String url = "jdbc:mysql://localhost:3306/db1";
        //链接数据库
        Connection connect = driver.connect(url, properties);

        System.out.println(connect);

    }
```

### 🔹方法③

```java
public static void main(String[] args) throws SQLException {
        //创建驱动
        Driver driver = new Driver();
        String url="jdbc:mysql://localhost:3306/db1";
        String password = "123456";
        String user = "root";
        //注册驱动
        DriverManager.registerDriver(driver);
        //链接数据库
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println(connection);
        
    }
```

### 🔹方法④

```java
public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url="jdbc:mysql://localhost:3306/db1";
        String password = "123456";
        String user = "root";
        Class.forName("com.mysql.jdbc.Driver");
        //无需注册驱动
        //DriverManager.registerDriver(driver);
        //链接数据库
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println(connection);
    }
```

### 🔹方法⑤

```java
public static void main(String[] args) throws IOException, ClassNotFoundException, SQLException {
        //读取配置文件
        InputStream is = ConnectionDemo5.class.getClassLoader().getResourceAsStream("jdbc.properties");

        Properties properties = new Properties();
        properties.load(is);

        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String classDriver = properties.getProperty("classDriver");
        //加载驱动（自动注册）
        Class.forName(classDriver);
        //链接数据库
        Connection connection = DriverManager.getConnection(url, user, password);

        System.out.println(connection);

    }
```

## 🔸数据【增删改】通用

```java
/**
     * 通用增删改
     * @param args
     * @throws Exception
     */
    public static void main(String[] args) throws Exception {
        //定义sql语句
        String sql = "update emp set ename=? where id = ?";
        //传参
        ModifyTheDatabase(sql,"xixixi",13);
    }

    /**
     * 通用增删改方法实现
     * @param sql
     * @param args
     * @throws Exception
     */
    public static void ModifyTheDatabase(String sql,Object ...args) throws Exception {
        //连接数据库
        Connection connection = JDBCUtils.getConnection();
        //准备sql语句
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //设置sql语句
        for (int i = 0; i < args.length; i++) {
            preparedStatement.setObject(i+1,args[i]);
        }
        //执行
        preparedStatement.execute();
        //释放资源
        JDBCUtils.colseResource(connection,preparedStatement);

    }
```

## 🔸数据【单表查询】通用

```java
public static void main(String[] args) throws Exception {
        Emp result = null;
        try {
            //调用查询方法并传递？被替换后的参数
            result = queryToObject("select ename from emp where id = ?", 12);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(result);
    }

    private static Emp queryToObject(String sql, Object... args) throws Exception {
        //链接数据库
        Connection conn = JDBCUtils.getConnection();
        //日常准备sql
        PreparedStatement ps = conn.prepareStatement(sql);
        //为每个？替换参数
        for (int i = 0; i < args.length; i++) {
            ps.setObject(i + 1, args[i]);
        }
        //执行sql并且返回结果
        ResultSet resultSet = ps.executeQuery();
        //得到表的元数据
        ResultSetMetaData metaData = resultSet.getMetaData();
        //得到表有多少列
        int columnCount = metaData.getColumnCount();

        if (resultSet.next()) {
            Emp emp = new Emp();
            String columnName = null;
            for (int i = 0; i < columnCount; i++) {
                //得到对应列的值
                Object value = resultSet.getObject(i + 1);
                //得到列的名称
                columnName = metaData.getColumnName(i + 1);
                //通过反射得到Emp对应的类名的属性
                Field declaredField = Emp.class.getDeclaredField(columnName);
                //强制打开属性权限
                declaredField.setAccessible(true);
                //设置改属性的值为得到的对应列的值
                declaredField.set(emp, value);
            }
            //释放资源
            JDBCUtils.colseResource(conn, ps);
            //返回该对象
            return emp;

        }
        return null;

    }
```

## 🔸数据【多表查询】通用

```java
/**
     * 通用表查询
     *
     * @param args
     */
    public static void main(String[] args) {
        try {
            //调用查询方法并传递？被替换后的参数
            Emp emp = queryToObject(Emp.class, "select * from emp where id = ?", 12);
            System.out.println(emp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 通用表查询实现
     *
     * @param clazz
     * @param sql
     * @param args
     * @param <T>
     * @return
     * @throws Exception
     */
    public static <T> T queryToObject(Class<T> clazz, String sql, Object... args) throws Exception {
        //链接数据库
        Connection conn = JDBCUtils.getConnection();
        //日常准备sql
        PreparedStatement ps = conn.prepareStatement(sql);
        //为每个？替换参数
        for (int i = 0; i < args.length; i++) {
            ps.setObject(i + 1, args[i]);
        }
        //执行sql并且返回结果
        ResultSet resultSet = ps.executeQuery();
        //得到表的元数据
        ResultSetMetaData metaData = resultSet.getMetaData();
        //得到表有多少列
        int columnCount = metaData.getColumnCount();

        if (resultSet.next()) {
            //通过反射来创建对象
            T t = clazz.newInstance();
            String columnName = null;
            for (int i = 0; i < columnCount; i++) {
                //得到对应列的值
                Object value = resultSet.getObject(i + 1);
                //得到列的名称
                columnName = metaData.getColumnName(i + 1);
                //通过反射得到Emp对应的类名的属性
                Field declaredField = Emp.class.getDeclaredField(columnName);
                //强制打开属性权限
                declaredField.setAccessible(true);
                //设置改属性的值为得到的对应列的值
                declaredField.set(t, value);
            }
            //释放资源
            JDBCUtils.colseResource(conn, ps);
            //返回该对象
            return t;

        }
        return null;

    }
```

## 🔸JDBCUtils工具类

```java
public class JDBCUtils {
    /**
     * 数据库链接实现方法
     *
     * @return
     * @throws Exception
     */
    public static Connection getConnection() throws Exception {
        //读取配置文件
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
        //加载配置文件到properties
        Properties properties = new Properties();
        properties.load(is);
        //获取文件对应内容
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        //链接数据库
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 资源释放方法实现
     *
     * @param connection
     * @param statement
     */

    public static void colseResource(Connection connection, Statement statement) {
        try {
            if (connection != null) {
                connection.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (statement != null) {
                statement.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

# Mybatis框架

> Mybatis官网：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

## 🔸初始准备

### 🔹1.加入Mybatis依赖

```
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.9</version>
</dependency>
```

### 🔹2.加入Mysql依赖

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
```

### 🔹3.加入Junit依赖

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

### 🔹4.创建并且配置mybatis-config.xml

[alert class="danger"]该文件用于配置数据库链接的基本信息[/alert]

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///db1?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

### 🔹5.创建并且配置UserMapper.xml

> 该文件用于配置sql语句

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="test">
    <select id="selectAll" resultType="blog.sky.tomcatweb.Mybatis.User">
        select * from user;
    </select>
</mapper>
```

## 🔸代码实现

### 🔹基本实现代码

> [推荐用Mapper做代理减少硬编码](https://www.bilibili.com/video/BV1Qf4y1T7Hx?p=50)

```java
public class MybatisDemo {
    public static void main(String[] args) throws IOException {
        //加载配置文件
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //执行UserMapper中的sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //填写UserMapper下的sql（推荐用Mapper做代理减少硬编码）
        List<User> users = sqlSession.selectList("test.selectAll");
        System.out.println(users);
        sqlSession.close();
    }
}
```

### 🔹使用Mapper代理

**过程：**

- ①定义与SQL映射文件同名的Mapper接口
- ②将接口和SQL映射文件放同一目录
- ③设置SQL映射文件的namespace为Mapper接口的路径
- ④在Mapper接口中定义方法。方法名=SQL映射文件中sql语句的id，并且保持参数类型和返回值一致
- ⑤通过sqlSection的getMapper方法获取Mapper接口对象，然后执行

***模块结构如图所示***

![img](PictureFile/【java】入坑笔记.assets/20220423145103.png)

> **Test部分主要代码**

```java
public class TestAll {
    @Test
    public void TestSelectAll() throws IOException {
        //加载配置文件
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //工厂获取sqlSeccion对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //使用Mapper代理，获取Mapper接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //执行接口中的方法（对应的sql）
        List<User> users = userMapper.selectAll();
        //打印输出
        System.out.println(users);
        //释放资源
        sqlSession.commit();
    }
}
```

# MyBatisPlus从入门到实践

## 🔸简介

[MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis (opens new window)](https://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**官网**：[MyBatis-Plus (baomidou.com)](https://baomidou.com/)

## 🔸特点

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 🔸框架结构

![framework](PictureFile/【java】入坑笔记.assets/mybatis-plus-framework.jpg)

## 🔸入门案例

- **SpringBoot整合Mybatis-plus**

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.2</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```

- **配置yml数据源信息**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db1
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource
```

- **Dao层**

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import skyblog.domain.User;

@Mapper
public interface UserDao extends BaseMapper<User> { //自动生成sql无需手写
}
```

- **测试**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;

import java.util.List;

@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getAll() {
        User user = userDao.selectById(1);
        System.out.println(user);
    }
}
```

##   🔸查询

[条件构造器 | MyBatis-Plus (baomidou.com)](https://baomidou.com/pages/10c804/#abstractwrapper)

### 🔹分页查询

- **添加Page拦截器**

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        //定义Mp拦截器
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        //拦截器里面添加Page拦截器
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return  mybatisPlusInterceptor;
    }
}
```

- **测试**

```java
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;

@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        Page page = new Page(1,2);
        userDao.selectPage(page,null);
        System.out.println("当前页码："+page.getCurrent());
        System.out.println("每页显示的数目："+page.getSize());
        System.out.println("一共有多少页："+page.getPages());
        System.out.println("一个有多少条数据："+page.getTotal());
        System.out.println("数据："+page.getRecords());
    }
}
```

- **输出**

```xml
当前页码：1
每页显示的数目：2
一共有多少页：11
一个有多少条数据：22
数据：[User{id=1, username='UpdateTest', password='123456'}, User{id=4, username='people2', password='123'}]
```

### 🔹等匹配查询

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;
@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        //匹配用户名和密码
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        lqw.eq(User::getUsername,"sky666").eq(User::getPassword,"123456");
        System.out.println(userDao.selectOne(lqw));
    }
}
```

### 🔹范围查询

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;
@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        //范围查询
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        lqw.between(User::getId,2,10);
        System.out.println(userDao.selectList(lqw));
    }
}
```

### 🔹模糊查询

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;
@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        //模糊查询
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        lqw.like(User::getUsername,"sky");
        System.out.println(userDao.selectList(lqw));
    }
}
```

## 🔸删除

### 🔹普通删除

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import java.util.ArrayList;

@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        ArrayList<Long> longs = new ArrayList<>();
        longs.add(1550033459488088066L);
        longs.add(1550034549189177346L);
        userDao.deleteBatchIds(longs);//多项删除
    }
}
```

### 🔹逻辑删除

- **添加逻辑删除字段**

> 通过写yml配置文件亦可

```java
@TableName("users")
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String username;
    private String password;
    @TableLogic(value = "0",delval = "1")//设置逻辑删除字段
    private Integer deleted;
}
```

- **测试**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;

@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        userDao.deleteById(1);
    }
}
```

- **结果**

```xml
==>  Preparing: UPDATE users SET deleted=1 WHERE id=? AND deleted=0
==> Parameters: 1(Integer)
<==    Updates: 1
```

## 🔸映射

<img src="PictureFile/【java】入坑笔记.assets/image-20220721151704599.png" alt="image-20220721151704599" style="zoom:80%;" />

### 🔹字段映射

```java
@TableName("users")//表名映射
public class User {
    private Integer id;
    private String username;
    @TableField(value = "password",select = false)//字段映射，不参与查询
    private String password;
    @TableField(exist = false)//设置为不存在
    private String Online;
    }
```

### 🔹表名映射

> **@TableName("users")//表名映射**

## 🔸ID生成策略

- #### domain中做配置

```java
@TableName("users")
public class User {
    @TableId(type = IdType.ASSIGN_ID)//雪花算法
    private Long id;
    private String username;
    private String password;
    }
```

- #### 测试

```java
mport org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;
@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        User user = new User(null,"sky","555555");
        userDao.insert(user);
    }
}
```

- #### 结果

```xml
==> Preparing: INSERT INTO users ( id, username, password ) VALUES ( ?, ?, ? )
==> Parameters: 1550034549189177346(Long), sky(String), 555555(String)
```

## 🔸锁

### 🔹乐观锁

- **添加乐观锁字段**

```java
@TableName("users")
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String username;
    private String password;
    @TableLogic(value = "0",delval = "1")
    private Integer deleted;
    @Version//乐观锁字段
    private Integer version;
}
```

- **添加拦截器**

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        //定义Mp拦截器
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        //拦截器里面添加Page拦截器
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        //添加乐观锁拦截器
        mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return  mybatisPlusInterceptor;
    }
}
```

- **测试**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;
import skyblog.domain.User;

@SpringBootTest
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        //模拟两个用户同时访问同一个数据
        User user1 = userDao.selectById(4);
        User user2 = userDao.selectById(4);

        user1.setUsername("user1");
        userDao.updateById(user1);

        user2.setUsername("user2");
        userDao.updateById(user2);
    }
}
```

- **结果**

```xml
==>  Preparing: SELECT id,username,password,deleted,version FROM users WHERE id=? AND deleted=0
==> Parameters: 4(Integer)
<==    Columns: id, username, password, deleted, version
<==        Row: 4, people2, 123, 0, 1
<==      Total: 1

==>  Preparing: SELECT id,username,password,deleted,version FROM users WHERE id=? AND deleted=0
==> Parameters: 4(Integer)
<==    Columns: id, username, password, deleted, version
<==        Row: 4, people2, 123, 0, 1
<==      Total: 1

==>  Preparing: UPDATE users SET username=?, password=?, version=? WHERE id=? AND version=? AND deleted=0
==> Parameters: user1(String), 123(String), 2(Integer), 4(Long), 1(Integer)
<==    Updates: 1

==>  Preparing: UPDATE users SET username=?, password=?, version=? WHERE id=? AND version=? AND deleted=0
==> Parameters: user2(String), 123(String), 2(Integer), 4(Long), 1(Integer)
<==    Updates: 0
```

- **原理解释**

> 当两个用户同时访问同一个数据时，只有第一个用户能够修改成功，因为当第二个用户访问时。version的值已经发生改变，所以修改操作无法找到相应的数据，导致修改失败。

## 🔸代码生成器

https://www.bilibili.com/video/BV12T4y1B7C3?p=14&t=846.1

#  Mysql【基础篇】

## SQL分类

| 分类 | 全称                       | 说明                                                  |
| ---- | -------------------------- | ----------------------------------------------------- |
| DDL  | Data Definition Language   | 数据定义语言,用来定义数据库对象（数据库,表,字段）     |
| DML  | Data Manipulation Language | 数据操作语言,用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language        | 数据查询语言,用来查询数据库中的表记录                 |
| DCL  | Data Contol Language       | 数据控制语言,用来创建数据库用户、控制数据库的访问权限 |

## 数据类型

### 整数类型

| 类型           | 大小（字节） | 有符号数取值范围                | 无符号数取值范围   | 说明     |
| -------------- | ------------ | ------------------------------- | ------------------ | -------- |
| TINYINT        | 1            | (-128, 127)                     | (0, 255)           | 超小整数 |
| SMALLINT       | 2            | (-32 768, 32 767)               | (0, 65 535)        | 小整数   |
| MEDIUMINT      | 3            | (-8 388 608, 8 388 607)         | (0, 16 777 215)    | 中等整数 |
| INT 或 INTEGER | 4            | (-2 147 483 648, 2 147 483 647) | (0, 4 294 967 295) | 整数     |
| BIGINT         | 8            | (-263, 263-1)                   | (0, 264-1 )        | 大整数   |

### 小数类型

| 类型             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| FLOAT(size, d)   | 单精度浮点数类型,4 个字节大小。size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。 |
| FLOAT(p)         | 单精度浮点数类型,参数 p 用来决定使用 FLOAT 类型还是 DOUBLE 类型：如果 p 的取值介于 0 和 24 之间,那么数据类型将变成 FLOAT()；如果 p 的取值介于 25 和 53 之间,那么数据类型将变成 DOUBLE()。 |
| DOUBLE(size, d)  | 双精度浮点数类型,8个字节大小。size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。 |
| DECIMAL(size, d) | 定点数类型,size 参数用来指定数字的总个数,d 参数用来指定小数部分（小数点后边）的数字个数。size 的最大值是 65,默认值是 10；d 的最大取值是 30,默认值是 0。 |
| DEC(size, d)     | 等价于 DECIMAL(size, d)。                                    |

### 字符串类型

| 类型                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| CHAR(size)                 | 用于表示固定长度的字符串,该字符串可以包含数字、字母和特殊字符。size 的大小可以是从 0 到 255 个字符,默认值为 1。 |
| VARCHAR(size)              | 用于表示可变长度的字符串,该字符串可以包含数字、字母和特殊字符。size 的大小可以是从 0 到 65535 个字符。 |
| TINYTEXT                   | 表示一个最大长度为 255（2 8-1）的字符串文本。                |
| TEXT(size)                 | 表示一个最大长度为 65,535（216-1）的字符串文本,也即 64KB。   |
| MEDIUMTEXT                 | 表示一个最大长度为 16,777,215（224-1）的字符串文本,也即 16MB。 |
| LONGTEXT                   | 表示一个最大长度为 4,294,967,295（232-1）的字符串文本,也即 4GB。 |
| ENUM(val1, val2, val3,...) | 字符串枚举类型,最多可以包含 65,535 个枚举值。插入的数据必须位于列表中,并且只能命中其中一个值；如果不在,将插入一个空值。 |
| SET( val1,val2,val3,....)  | 字符串集合类型,最多可以列出 64 个值。插入的数据可以命中其中的一个或者多个值,如果没有命中,将插入一个空值。 |

### 日期时间类型

| 类型           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| DATE           | 日期类型,格式为 YYYY-MM-DD,取值范围从 '1000-01-01' 到 '9999-12-31'。 |
| DATETIME(fsp)  | 日期和时间类型,格式为 YYYY-MM-DD hh:mm:ss,取值范围从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'。 |
| TIMESTAMP(fsp) | 时间戳类型,它存储的值为从 Unix 纪元（'1970-01-01 00:00:00' UTC）到现在的秒数。TIMESTAMP 的格式为 YYYY-MM-DD hh:mm:ss,取值范围从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC。 |
| TIME(fsp)      | 时间类型,格式为 hh:mm:ss,取值范围从 '-838:59:59' 到 '838:59:59'。 |
| YEAR           | 四位数字的年份格式,允许使用从 1901 到 2155 之间的四位数字的年份。此外,还有一个特殊的取值,就是 0000。 |

### 二进制类型

| 类型            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| BIT(size)       | 二进制位（Bit）类型,位数由 size 参数指定；size 的大小从 1 到 64,默认值为 1。 |
| BINARY(Size)    | 等价于 CHAR() 类型,但是存储的是二进制形式的字节串。size 参数以字节（Byte）为单位指定列的长度,默认值为1。 |
| VARBINARY(Size) | 等价于 VARCHAR() 类型,但是存储的是二进制形式的字节串。size 参数以字节（Byte）为单位指定列的最大长度。 |
| TINYBLOB        | 存储较小的二进制数据,最多可容纳 255 (28-1)个字节。           |
| BLOB(size)      | 用来储存二进制数据,最多可以容纳 65,535（216-1）个字节,也即 64KB。 |
| MEDIUMBLOB      | 存储中等大小的二进制数据,最多可以容纳 16,777,215（224-1）字节,也即 16MB。 |
| LONGBLOB        | 存储较大的二进制数据,最多可容纳 42,94,967,295（232-1）字节,也即 4GB。 |

## Data Definition Language

### 数据库操作

```sql
#查询所有数据库
SHOW DATABASES; 

#查询当前数据库
SELECT DATABASES(); 

#创建数据库
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]

#删除数据库
DROP DATABASE[IF EXISTS]数据库名

#使用数据库
USE 数据库名
```

### 表操作

```sql
#查询所有表
SHOW TABLES;

#查询表结构
DESC 表名;

#查询建表语句
SHOW CREATE TABLE 表名;

#删除表
DROP TABLE 表名;

#创建表
create table user_test(
    -> id int comment '编号',
    -> name varchar(50) comment '姓名',
    -> age int comment '年龄',
    -> gender varchar(1) comment '性别'
    -> ) comment '用户表';
```

### 字段操作

```sql
#添加字段
ALTER TABLE 表名 ADD 字段名 类型 [COMMENT 注释] [约束]

#修改数据类型
ALTER TABLE 表名 MODIFY 字段名 新的数据类型

#修改字段名和字段类型
ALTER TABLE 表名 CHANGE 旧的字段名 新的字段名 类型 [COMMENT 注释] [约束]
```

## Data Manipulation Language

### 插入数据

```sql
#给指定表中的指定字段添加数据
INSERT INTO 表名 (字段名1,字段名2,...) VALUES (值1,值2,...);

#给指定表中的所有字段添加数据
INSERT INTO 表名 VALUES (值1,值2,...);

#批量添加数据
INSERT INTO 表名 (字段名1,字段名2,...) VALUES (值1,值2,...),(值1,值2,...);
INSERT INTO 表名 VALUES (值1,值2,...),(值1,值2,...);
```

### 修改数据

```sql
#修改指定字段值
UPDATA 表名 SET 字段名1 = 值1,字段名1 = 值1 [WHERE 条件语句];

#修改所有字段值！！！
UPDATA 表名 SET 字段名1 = 值1,字段名1 = 值1;
```

### 删除数据

```sql
#删除指定数据
DELETE FROM 表名 [WHRER 条件];

#删除所有数据
DELETE FROM 表名;
```

## Data Query Language

### 基础查询

```sql
#查询指定字段数据
SELECT 字段名1,字段名2 FROM 表名;

#查询表中所有数据
SELECT * from 表名;

#查询指定字段数据(不重复)
SELECT DISTINCT 字段名1,字段名2 from 表名;

#查询框架
SELECT 字段列表
FROM 表名
WHERE 条件
GROUP BY 分组字段
HAVING 分组过滤条件
ORDER BY 排序字段
LIMIT 分页参数

执行顺序：
FROM
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
LIMIT
```

### 条件查询

| 操作符           | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| =                | 等号，检测两个值是否相等，如果相等返回true                   |
| <> 或 !=         | 不等于，检测两个值是否相等，如果不相等返回true               |
| >                | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true |
| <                | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true |
| >=               | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true |
| <=               | 小于等于号，检测左边的值是否小于于或等于右边的值, 如果左边的值小于或等于右边的值返回true |
| BETWEEN...AND... | 在某个范围之内(包含边界)                                     |
| IN(...)          | 在in之后的列表中的值，多选一                                 |
| LIKE 占位符      | 模糊匹配(_匹配单个字符，%匹配任意个字符)                     |
| IS NULL          | 是NULL                                                       |
| AND 或 &&        | 并且                                                         |
| OR  或 \|\|      | 或者                                                         |
| NOT 或 ！        | 非                                                           |

```sql
#条件查询
SELECT 字段名1，字段名2，... FROM 表名 WHERE 条件

例子：
select * 
from user_test 
where age <= 18;

select * 
from user_test 
where age between 15 and 18;

select *
from user_test
where age in(17,18,19);

#查询所有名字是四个字符的数据
select *
from user_test
where name like '____';

#查询年龄以0结尾的数据
select *
from user_test
where age like '%0';
```

### 聚合查询

```sql
例子：
#统计所有name字段的数目
select 
count(name) 
from user_test;

#统计表中age的平均数
select 
avg(age) 
from user_test;

#统计表中age的最大值
select 
max(age) 
from user_test;

#统计表中age的最小值
select 
min(age) 
from user_test;

#统计性别为男性的年龄总和
select 
sum(age) 
from user_test 
where gender = '1' or gender = '男';
```

### 分组查询

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 字段名 [HAVING 分组过滤条件]
#PS：where是分组之前过滤，having是分组之后过滤

例子：
#统计男女分别有多少条数据
select gender, count(*)
from user_test
group by gender;

#统计男女分别的平均年龄
select gender, avg(age)
from user_test
group by gender;

#统计年龄小于等于30岁的数据男女分别有多少条，并且保留统计结果大于5的记录
select gender, count(*)
from user_test
where age <= 30
group by gender
having count(*) > 5;
```

###  排序查询

```sql
SELECT 字段名 FROM 表名 ORDER BY 字段名 排序方式
#PS：排序方式：asc 升序 desc降序

#根据年龄进行降序排序
select *
from user_test
order by age desc;

#先根据name进行升序排序，在根据age进行降序排序
select *
from user_test
order by name asc,age desc;
```

### 分页查询

```sql
SELECT 字段列表 FROM 表名 LIMIT 其实索引，查询记录数；
#PS：起始索引=（查询页码-1）*每页记录数

#查询其实索引为0，每页10条数据
select *
from user_test
limit 0,10;
```

### 多表查询

#### 内连接查询

> 内连接查询是两张表的交集部分

```sql
#隐式内连接
SELECT 字段列表 FROM 表1，表2 WHERE 条件...
例子：
select *
from table2,
     dept
where dept.id = table2.dept_id;

#显式内连接
SELECT 字段列表 FROM 表1 [INNER]JOIN 表2 ON 连接条件...
例子：
select *
from table2
         inner join dept on table2.dept_id = dept.id;
```

#### 外连接查询

> 相当于查询表1（左表）的所有数据 包含 表1和表2交集部分的数据

```sql
#左外连接
SELECT 字段列表 FROM 表1 LEFT [OUTER]JOIN 表2 ON 条件
例子：
select table2.*, dept.*
from table2
         left outer join dept on table2.dept_id = dept.id;
```

> 相当于查询表2（右表）的所有数据 包含 表1和表2交集部分的数据

```sql
#右外连接
SELECT 字段列表 FROM 表1 RIGHT [OUTER]JOIN 表2 ON 条件
例子： 
select dept.*, table2.*
from table2
         right outer join dept on table2.dept_id = dept.id;
```

#### 自连接查询

> 自连接可以是内连接也可以是外连接

```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...
```

#### 联合查询

```sql
#联合查询
SQL语句1
UNION[ALL]
SQL语句2;

例子：去重
select * from table2 where age < 50
union
select * from table2 where gender = 'F';

例子：不去重
select * from table2 where age < 50
union all
select * from table2 where gender = 'F';
```

#### 子查询

- 标量子查询

> 子查询结果返回单个值

```sql
#查询department为aa的table2表的所有数据
select *
from table2
where dept_id = (select id from dept where department = 'aa');
```

- 列子查询

> 子查询结果为一列

```sql
#查询department为aa或的table2表的所有数据
select *
from table2
where dept_id in (select id from dept where department = 'aa' or department = 'bb');
```

- 行子查询

> 子查询结果为一行

```sql
select *
from emp
where (salary, managerid) = (select salary, managerid from emp where name = 'sky');
```

- 表子查询

> 子查询结果为多行多列

```sql
select *
from emp
where (job, salary) in (select job, salary from emp where name = 'sky' or name = 'shabo');
```

##   Data Contol Language

### 用户管理

```sql
#创建用户 %代表所有主机都可以访问
create user 'sky'@'localhost' identified by '123456';
create user 'sky'@'%' identified by '123456';

#修改用户密码
set password for 'sky6'@'localhost' = password('666');

#删除用户
drop user 'sky'@'localhost';
```

### 权限控制

```sql
#查询权限
SHOW GRANTS FOR '用户名'@'主机名';
show grants for 'sky'@'localhost';

#授予权限
GRANT ALL ON 数据库名.表名 TO '用户名'@'主机名';
grant all on test1.* to 'sky'@'localhost';

#撤销权限
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
revoke all on test1.* from 'sky'@'localhost';
```

## 约束

### 关键字约束

| 约束               | 描述                                                   | 关键字      |
| ------------------ | ------------------------------------------------------ | ----------- |
| 非空约束           | 现在字段数据不能为null                                 | NOT NULL    |
| 唯一约束           | 保证字段所有数据唯一、不重复                           | UNIQUE      |
| 主键约束           | 主键是一行数据的唯一标识，要求非空且唯一               | PRIMARY KEY |
| 默认约束           | 保存数据是，如果未指定该字段的值，则采用默认值         | DEFAULT     |
| 检查约束（8.0.16） | 保证字段值满足某一个条件                               | CHECK       |
| 外键约束           | 用来让两张表数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |

```sql
create table table2 (
    id int primary key auto_increment comment 'id',
    name varchar(20) not null unique comment '姓名',
    age int comment '年龄',
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
);
```

### 外键约束

```sql
#添加外键
ALTER TABLE 表名 
ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
ON UPDATE 行为 ON DELETE 行为
例子：
alter table table2 add constraint fk_table2_dept_id foreign key (dept_id) references dept(id);

CONSTRAINT子句允许您为外键约束定义约束名称。如果省略它，MySQL将自动生成一个名称。
FOREIGN KEY子句指定子表中引用父表中主键列的列。您可以在FOREIGN KEY子句后放置一个外键名称，或者让MySQL为您创建一个名称。 请注意，MySQL会自动创建一个具有foreign_key_name名称的索引。
REFERENCES子句指定父表及其子表中列的引用。 在FOREIGN KEY和REFERENCES中指定的子表和父表中的列数必须相同。

#删除外键
ALTER TABLE 表名
DROP FOREIGN KEY 外键名称
```

### 行为补充

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新 |
| RESTRICT    | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新 |
| CASCADE     | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录 |
| SET NULL    | 当在父表中删除对应记录是，首先检查该记录是否有对应外键，如果有则设置子表中该外键的值为null |
| SET DEFAULT | 父表有变更时，子表将外键列设置成一个默认的值（innodb不支持） |

```sql
#添加外键
ALTER TABLE 表名 
ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
ON UPDATE 行为 ON DELETE 行为
```

## 事物

### 事物特性

| 特性       | 描述                                                     |
| ---------- | -------------------------------------------------------- |
| **原子性** | 是不可分割的最小的操作单位，要么同时成功，要么同时失败。 |
| **持久性** | 当事务提交或回滚后，数据库会持久化的保存数据。           |
| **隔离性** | 多个事务之间。相互独立。                                 |
| **一致性** | 多个事务之间。相互独立。                                 |

### 事物操作

- 方式1

```sql
#设置当前会话取消自动提交事物
set @@autocommit = 0;

update account set money = money - 500 where name = '张三';
update account set money = money + 500 where name = '李四';

#提交事物
commit ;

#事物回滚（清空缓存区）
rollback ;
```

- 方式2

```sql
#开始事物
start transaction ;

update account set money = money - 500 where name = '张三';
update account set money = money + 500 where name = '李四';
#提交事物
commit ;

#事物回滚（清空缓存区）
rollback ;
```

### 并发事物问题

| 问题           | 描述                                   |
| -------------- | -------------------------------------- |
| **脏读**       | 一个事务读取了另一个事务未提交的数据。 |
| **不可重复读** | 一个事务前后两次读取的同一数据不一致。 |
| **幻读**       | 一个事务两次查询的结果集记录数不一致。 |

### 事务隔离级别

| 隔离级别                     | 脏读 | 不可重复读 | 幻读 | 隔离性 | 并发性 |
| :--------------------------- | :--: | :--------: | :--: | :----: | :----: |
| 顺序读（SERIALIZABLE）       |  N   |     N      |  N   |  最高  |  最低  |
| 可重复读（REPEATABLE READ）  |  N   |     N      |  Y   |        |        |
| 读以提交（READ COMMITTED）   |  N   |     Y      |  Y   |        |        |
| 读未提交（READ UNCOMMITTED） |  Y   |     Y      |  Y   |  最低  |  最高  |

```sql
#查看事物隔离界别
select @@transaction_isolation;

#设置事物隔离级别		
set session transaction isolation level repeatable read ;
```



## 内置函数

### 字符串函数

| 函数                                  | 描述                                                         | 实例                                                         |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ASCII(s)                              | 返回字符串 s 的第一个字符的 ASCII 码。                       | 返回 CustomerName 字段第一个字母的 ASCII 码：`SELECT ASCII(CustomerName) AS NumCodeOfFirstChar FROM Customers;` |
| CHAR_LENGTH(s)                        | 返回字符串 s 的字符数                                        | 返回字符串 RUNOOB 的字符数`SELECT CHAR_LENGTH("RUNOOB") AS LengthOfString;` |
| CHARACTER_LENGTH(s)                   | 返回字符串 s 的字符数，等同于 CHAR_LENGTH(s)                 | 返回字符串 RUNOOB 的字符数`SELECT CHARACTER_LENGTH("RUNOOB") AS LengthOfString;` |
| CONCAT(s1,s2...sn)                    | 字符串 s1,s2 等多个字符串合并为一个字符串                    | 合并多个字符串`SELECT CONCAT("SQL ", "Runoob ", "Gooogle ", "Facebook") AS ConcatenatedString;` |
| CONCAT_WS(x, s1,s2...sn)              | 同 CONCAT(s1,s2,...) 函数，但是每个字符串之间要加上 x，x 可以是分隔符 | 合并多个字符串，并添加分隔符：`SELECT CONCAT_WS("-", "SQL", "Tutorial", "is", "fun!")AS ConcatenatedString;` |
| FIELD(s,s1,s2...)                     | 返回第一个字符串 s 在字符串列表(s1,s2...)中的位置            | 返回字符串 c 在列表值中的位置：`SELECT FIELD("c", "a", "b", "c", "d", "e");` |
| FIND_IN_SET(s1,s2)                    | 返回在字符串s2中与s1匹配的字符串的位置                       | 返回字符串 c 在指定字符串中的位置：`SELECT FIND_IN_SET("c", "a,b,c,d,e");` |
| FORMAT(x,n)                           | 函数可以将数字 x 进行格式化 "#,###.##", 将 x 保留到小数点后 n 位，最后一位四舍五入。 | 格式化数字 "#,###.##" 形式：`SELECT FORMAT(250500.5634, 2);     -- 输出 250,500.56` |
| INSERT(s1,x,len,s2)                   | 字符串 s2 替换 s1 的 x 位置开始长度为 len 的字符串           | 从字符串第一个位置开始的 6 个字符替换为 runoob：`SELECT INSERT("google.com", 1, 6, "runoob");  -- 输出：runoob.com` |
| LOCATE(s1,s)                          | 从字符串 s 中获取 s1 的开始位置                              | 获取 b 在字符串 abc 中的位置：`SELECT LOCATE('st','myteststring');  -- 5`返回字符串 abc 中 b 的位置：`SELECT LOCATE('b', 'abc') -- 2` |
| LCASE(s)                              | 将字符串 s 的所有字母变成小写字母                            | 字符串 RUNOOB 转换为小写：`SELECT LCASE('RUNOOB') -- runoob` |
| LEFT(s,n)                             | 返回字符串 s 的前 n 个字符                                   | 返回字符串 runoob 中的前两个字符：`SELECT LEFT('runoob',2) -- ru` |
| LOWER(s)                              | 将字符串 s 的所有字母变成小写字母                            | 字符串 RUNOOB 转换为小写：`SELECT LOWER('RUNOOB') -- runoob` |
| LPAD(s1,len,s2)                       | 在字符串 s1 的开始处填充字符串 s2，使字符串长度达到 len      | 将字符串 xx 填充到 abc 字符串的开始处：`SELECT LPAD('abc',5,'xx') -- xxabc` |
| LTRIM(s)                              | 去掉字符串 s 开始处的空格                                    | 去掉字符串 RUNOOB开始处的空格：`SELECT LTRIM("    RUNOOB") AS LeftTrimmedString;-- RUNOOB` |
| MID(s,n,len)                          | 从字符串 s 的 n 位置截取长度为 len 的子字符串，同 SUBSTRING(s,n,len) | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT MID("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| POSITION(s1 IN s)                     | 从字符串 s 中获取 s1 的开始位置                              | 返回字符串 abc 中 b 的位置：`SELECT POSITION('b' in 'abc') -- 2` |
| REPEAT(s,n)                           | 将字符串 s 重复 n 次                                         | 将字符串 runoob 重复三次：`SELECT REPEAT('runoob',3) -- runoobrunoobrunoob` |
| REPLACE(s,s1,s2)                      | 将字符串 s2 替代字符串 s 中的字符串 s1                       | 将字符串 abc 中的字符 a 替换为字符 x：`SELECT REPLACE('abc','a','x') --xbc` |
| REVERSE(s)                            | 将字符串s的顺序反过来                                        | 将字符串 abc 的顺序反过来：`SELECT REVERSE('abc') -- cba`    |
| RIGHT(s,n)                            | 返回字符串 s 的后 n 个字符                                   | 返回字符串 runoob 的后两个字符：`SELECT RIGHT('runoob',2) -- ob` |
| RPAD(s1,len,s2)                       | 在字符串 s1 的结尾处添加字符串 s2，使字符串的长度达到 len    | 将字符串 xx 填充到 abc 字符串的结尾处：`SELECT RPAD('abc',5,'xx') -- abcxx` |
| RTRIM(s)                              | 去掉字符串 s 结尾处的空格                                    | 去掉字符串 RUNOOB 的末尾空格：`SELECT RTRIM("RUNOOB     ") AS RightTrimmedString;   -- RUNOOB` |
| SPACE(n)                              | 返回 n 个空格                                                | 返回 10 个空格：`SELECT SPACE(10);`                          |
| STRCMP(s1,s2)                         | 比较字符串 s1 和 s2，如果 s1 与 s2 相等返回 0 ，如果 s1>s2 返回 1，如果 s1<s2 返回 -1 | 比较字符串：`SELECT STRCMP("runoob", "runoob");  -- 0`       |
| SUBSTR(s, start, length)              | 从字符串 s 的 start 位置截取长度为 length 的子字符串         | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT SUBSTR("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| SUBSTRING(s, start, length)           | 从字符串 s 的 start 位置截取长度为 length 的子字符串，等同于 SUBSTR(s, start, length) | 从字符串 RUNOOB 中的第 2 个位置截取 3个 字符：`SELECT SUBSTRING("RUNOOB", 2, 3) AS ExtractString; -- UNO` |
| SUBSTRING_INDEX(s, delimiter, number) | 返回从字符串 s 的第 number 个出现的分隔符 delimiter 之后的子串。 如果 number 是正数，返回第 number 个字符左边的字符串。 如果 number 是负数，返回第(number 的绝对值(从右边数))个字符右边的字符串。 | `SELECT SUBSTRING_INDEX('a*b','*',1) -- a SELECT SUBSTRING_INDEX('a*b','*',-1)  -- b SELECT SUBSTRING_INDEX(SUBSTRING_INDEX('a*b*c*d*e','*',3),'*',-1)  -- c` |
| TRIM(s)                               | 去掉字符串 s 开始和结尾处的空格                              | 去掉字符串 RUNOOB 的首尾空格：`SELECT TRIM('    RUNOOB    ') AS TrimmedString;` |
| UCASE(s)                              | 将字符串转换为大写                                           | 将字符串 runoob 转换为大写：`SELECT UCASE("runoob"); -- RUNOOB` |
| UPPER(s)                              | 将字符串转换为大写                                           | 将字符串 runoob 转换为大写：`SELECT UPPER("runoob"); -- RUNOOB` |

### 数值函数

| 函数名                             | 描述                                                         | 实例                                                         |
| :--------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| ABS(x)                             | 返回 x 的绝对值                                              | 返回 -1 的绝对值：`SELECT ABS(-1) -- 返回1`                  |
| ACOS(x)                            | 求 x 的反余弦值（单位为弧度），x 为一个数值                  | `SELECT ACOS(0.25);`                                         |
| ASIN(x)                            | 求反正弦值（单位为弧度），x 为一个数值                       | `SELECT ASIN(0.25);`                                         |
| ATAN(x)                            | 求反正切值（单位为弧度），x 为一个数值                       | `SELECT ATAN(2.5);`                                          |
| ATAN2(n, m)                        | 求反正切值（单位为弧度）                                     | `SELECT ATAN2(-0.8, 2);`                                     |
| AVG(expression)                    | 返回一个表达式的平均值，expression 是一个字段                | 返回 Products 表中Price 字段的平均值：`SELECT AVG(Price) AS AveragePrice FROM Products;` |
| CEIL(x)                            | 返回大于或等于 x 的最小整数                                  | `SELECT CEIL(1.5) -- 返回2`                                  |
| CEILING(x)                         | 返回大于或等于 x 的最小整数                                  | `SELECT CEILING(1.5); -- 返回2`                              |
| COS(x)                             | 求余弦值(参数是弧度)                                         | `SELECT COS(2);`                                             |
| COT(x)                             | 求余切值(参数是弧度)                                         | `SELECT COT(6);`                                             |
| COUNT(expression)                  | 返回查询的记录总数，expression 参数是一个字段或者 * 号       | 返回 Products 表中 products 字段总共有多少条记录：`SELECT COUNT(ProductID) AS NumberOfProducts FROM Products;` |
| DEGREES(x)                         | 将弧度转换为角度                                             | `SELECT DEGREES(3.1415926535898) -- 180`                     |
| n DIV m                            | 整除，n 为被除数，m 为除数                                   | 计算 10 除于 5：`SELECT 10 DIV 5;  -- 2`                     |
| EXP(x)                             | 返回 e 的 x 次方                                             | 计算 e 的三次方：`SELECT EXP(3) -- 20.085536923188`          |
| FLOOR(x)                           | 返回小 于或等于 x 的最大整数                                 | 小于或等于 1.5 的整数：`SELECT FLOOR(1.5) -- 返回1`          |
| GREATEST(expr1, expr2, expr3, ...) | 返回列表中的最大值                                           | 返回以下数字列表中的最大值：`SELECT GREATEST(3, 12, 34, 8, 25); -- 34`返回以下字符串列表中的最大值：`SELECT GREATEST("Google", "Runoob", "Apple");   -- Runoob` |
| LEAST(expr1, expr2, expr3, ...)    | 返回列表中的最小值                                           | 返回以下数字列表中的最小值：`SELECT LEAST(3, 12, 34, 8, 25); -- 3`返回以下字符串列表中的最小值：`SELECT LEAST("Google", "Runoob", "Apple");   -- Apple` |
| LN                                 | 返回数字的自然对数，以 e 为底。                              | 返回 2 的自然对数：`SELECT LN(2);  -- 0.6931471805599453`    |
| LOG(x) 或 LOG(base, x)             | 返回自然对数(以 e 为底的对数)，如果带有 base 参数，则 base 为指定带底数。 | `SELECT LOG(20.085536923188) -- 3 SELECT LOG(2, 4); -- 2`    |
| LOG10(x)                           | 返回以 10 为底的对数                                         | `SELECT LOG10(100) -- 2`                                     |
| LOG2(x)                            | 返回以 2 为底的对数                                          | 返回以 2 为底 6 的对数：`SELECT LOG2(6);  -- 2.584962500721156` |
| MAX(expression)                    | 返回字段 expression 中的最大值                               | 返回数据表 Products 中字段 Price 的最大值：`SELECT MAX(Price) AS LargestPrice FROM Products;` |
| MIN(expression)                    | 返回字段 expression 中的最小值                               | 返回数据表 Products 中字段 Price 的最小值：`SELECT MIN(Price) AS MinPrice FROM Products;` |
| MOD(x,y)                           | 返回 x 除以 y 以后的余数                                     | 5 除于 2 的余数：`SELECT MOD(5,2) -- 1`                      |
| PI()                               | 返回圆周率(3.141593）                                        | `SELECT PI() --3.141593`                                     |
| POW(x,y)                           | 返回 x 的 y 次方                                             | 2 的 3 次方：`SELECT POW(2,3) -- 8`                          |
| POWER(x,y)                         | 返回 x 的 y 次方                                             | 2 的 3 次方：`SELECT POWER(2,3) -- 8`                        |
| RADIANS(x)                         | 将角度转换为弧度                                             | 180 度转换为弧度：`SELECT RADIANS(180) -- 3.1415926535898`   |
| RAND()                             | 返回 0 到 1 的随机数                                         | `SELECT RAND() --0.93099315644334`                           |
| ROUND(x [,y])                      | 返回离 x 最近的整数，可选参数 y 表示要四舍五入的小数位数，如果省略，则返回整数。 | `SELECT ROUND(1.23456) --1 SELECT ROUND(345.156, 2) -- 345.16` |
| SIGN(x)                            | 返回 x 的符号，x 是负数、0、正数分别返回 -1、0 和 1          | `SELECT SIGN(-10) -- (-1)`                                   |
| SIN(x)                             | 求正弦值(参数是弧度)                                         | `SELECT SIN(RADIANS(30)) -- 0.5`                             |
| SQRT(x)                            | 返回x的平方根                                                | 25 的平方根：`SELECT SQRT(25) -- 5`                          |
| SUM(expression)                    | 返回指定字段的总和                                           | 计算 OrderDetails 表中字段 Quantity 的总和：`SELECT SUM(Quantity) AS TotalItemsOrdered FROM OrderDetails;` |
| TAN(x)                             | 求正切值(参数是弧度)                                         | `SELECT TAN(1.75);  -- -5.52037992250933`                    |
| TRUNCATE(x,y)                      | 返回数值 x 保留到小数点后 y 位的值（与 ROUND 最大的区别是不会进行四舍五入） | `SELECT TRUNCATE(1.23456,3) -- 1.234`                        |

### 日期函数

| 函数名                                    | 描述                                                         | 实例                                                         |
| :---------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| ADDDATE(d,n)                              | 计算起始日期 d 加上 n 天的日期                               | `SELECT ADDDATE("2017-06-15", INTERVAL 10 DAY); ->2017-06-25` |
| ADDTIME(t,n)                              | n 是一个时间表达式，时间 t 加上时间表达式 n                  | 加 5 秒：`SELECT ADDTIME('2011-11-11 11:11:11', 5); ->2011-11-11 11:11:16 (秒)`添加 2 小时, 10 分钟, 5 秒:`SELECT ADDTIME("2020-06-15 09:34:21", "2:10:5");  -> 2020-06-15 11:44:26` |
| CURDATE()                                 | 返回当前日期                                                 | `SELECT CURDATE(); -> 2018-09-19`                            |
| CURRENT_DATE()                            | 返回当前日期                                                 | `SELECT CURRENT_DATE(); -> 2018-09-19`                       |
| CURRENT_TIME                              | 返回当前时间                                                 | `SELECT CURRENT_TIME(); -> 19:59:02`                         |
| CURRENT_TIMESTAMP()                       | 返回当前日期和时间                                           | `SELECT CURRENT_TIMESTAMP() -> 2018-09-19 20:57:43`          |
| CURTIME()                                 | 返回当前时间                                                 | `SELECT CURTIME(); -> 19:59:02`                              |
| DATE()                                    | 从日期或日期时间表达式中提取日期值                           | `SELECT DATE("2017-06-15");     -> 2017-06-15`               |
| DATEDIFF(d1,d2)                           | 计算日期 d1->d2 之间相隔的天数                               | `SELECT DATEDIFF('2001-01-01','2001-02-02') -> -32`          |
| DATE_ADD(d，INTERVAL expr type)           | 计算起始日期 d 加上一个时间段后的日期，type 值可以是：       | `SELECT DATE_ADD("2017-06-15", INTERVAL 10 DAY);     -> 2017-06-25 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL 15 MINUTE); -> 2017-06-15 09:49:21 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL -3 HOUR); ->2017-06-15 06:34:21 SELECT DATE_ADD("2017-06-15 09:34:21", INTERVAL -3 MONTH); ->2017-04-15` |
| DATE_FORMAT(d,f)                          | 按表达式 f的要求显示日期 d                                   | `SELECT DATE_FORMAT('2011-11-11 11:11:11','%Y-%m-%d %r') -> 2011-11-11 11:11:11 AM` |
| DATE_SUB(date,INTERVAL expr type)         | 函数从日期减去指定的时间间隔。                               | Orders 表中 OrderDate 字段减去 2 天：`SELECT OrderId,DATE_SUB(OrderDate,INTERVAL 2 DAY) AS OrderPayDate FROM Orders` |
| DAY(d)                                    | 返回日期值 d 的日期部分                                      | `SELECT DAY("2017-06-15");   -> 15`                          |
| DAYNAME(d)                                | 返回日期 d 是星期几，如 Monday,Tuesday                       | `SELECT DAYNAME('2011-11-11 11:11:11') ->Friday`             |
| DAYOFMONTH(d)                             | 计算日期 d 是本月的第几天                                    | `SELECT DAYOFMONTH('2011-11-11 11:11:11') ->11`              |
| DAYOFWEEK(d)                              | 日期 d 今天是星期几，1 星期日，2 星期一，以此类推            | `SELECT DAYOFWEEK('2011-11-11 11:11:11') ->6`                |
| DAYOFYEAR(d)                              | 计算日期 d 是本年的第几天                                    | `SELECT DAYOFYEAR('2011-11-11 11:11:11') ->315`              |
| EXTRACT(type FROM d)                      | 从日期 d 中获取指定的值，type 指定返回的值。 type可取值为：  | `SELECT EXTRACT(MINUTE FROM '2011-11-11 11:11:11')  -> 11`   |
| FROM_DAYS(n)                              | 计算从 0000 年 1 月 1 日开始 n 天后的日期                    | `SELECT FROM_DAYS(1111) -> 0003-01-16`                       |
| HOUR(t)                                   | 返回 t 中的小时值                                            | `SELECT HOUR('1:2:3') -> 1`                                  |
| LAST_DAY(d)                               | 返回给给定日期的那一月份的最后一天                           | `SELECT LAST_DAY("2017-06-20"); -> 2017-06-30`               |
| LOCALTIME()                               | 返回当前日期和时间                                           | `SELECT LOCALTIME() -> 2018-09-19 20:57:43`                  |
| LOCALTIMESTAMP()                          | 返回当前日期和时间                                           | `SELECT LOCALTIMESTAMP() -> 2018-09-19 20:57:43`             |
| MAKEDATE(year, day-of-year)               | 基于给定参数年份 year 和所在年中的天数序号 day-of-year 返回一个日期 | `SELECT MAKEDATE(2017, 3); -> 2017-01-03`                    |
| MAKETIME(hour, minute, second)            | 组合时间，参数分别为小时、分钟、秒                           | `SELECT MAKETIME(11, 35, 4); -> 11:35:04`                    |
| MICROSECOND(date)                         | 返回日期参数所对应的微秒数                                   | `SELECT MICROSECOND("2017-06-20 09:34:00.000023"); -> 23`    |
| MINUTE(t)                                 | 返回 t 中的分钟值                                            | `SELECT MINUTE('1:2:3') -> 2`                                |
| MONTHNAME(d)                              | 返回日期当中的月份名称，如 November                          | `SELECT MONTHNAME('2011-11-11 11:11:11') -> November`        |
| MONTH(d)                                  | 返回日期d中的月份值，1 到 12                                 | `SELECT MONTH('2011-11-11 11:11:11') ->11`                   |
| NOW()                                     | 返回当前日期和时间                                           | `SELECT NOW() -> 2018-09-19 20:57:43`                        |
| PERIOD_ADD(period, number)                | 为 年-月 组合日期添加一个时段                                | `SELECT PERIOD_ADD(201703, 5);    -> 201708`                 |
| PERIOD_DIFF(period1, period2)             | 返回两个时段之间的月份差值                                   | `SELECT PERIOD_DIFF(201710, 201703); -> 7`                   |
| QUARTER(d)                                | 返回日期d是第几季节，返回 1 到 4                             | `SELECT QUARTER('2011-11-11 11:11:11') -> 4`                 |
| SECOND(t)                                 | 返回 t 中的秒钟值                                            | `SELECT SECOND('1:2:3') -> 3`                                |
| SEC_TO_TIME(s)                            | 将以秒为单位的时间 s 转换为时分秒的格式                      | `SELECT SEC_TO_TIME(4320) -> 01:12:00`                       |
| STR_TO_DATE(string, format_mask)          | 将字符串转变为日期                                           | `SELECT STR_TO_DATE("August 10 2017", "%M %d %Y"); -> 2017-08-10` |
| SUBDATE(d,n)                              | 日期 d 减去 n 天后的日期                                     | `SELECT SUBDATE('2011-11-11 11:11:11', 1) ->2011-11-10 11:11:11 (默认是天)` |
| SUBTIME(t,n)                              | 时间 t 减去 n 秒的时间                                       | `SELECT SUBTIME('2011-11-11 11:11:11', 5) ->2011-11-11 11:11:06 (秒)` |
| SYSDATE()                                 | 返回当前日期和时间                                           | `SELECT SYSDATE() -> 2018-09-19 20:57:43`                    |
| TIME(expression)                          | 提取传入表达式的时间部分                                     | `SELECT TIME("19:30:10"); -> 19:30:10`                       |
| TIME_FORMAT(t,f)                          | 按表达式 f 的要求显示时间 t                                  | `SELECT TIME_FORMAT('11:11:11','%r') 11:11:11 AM`            |
| TIME_TO_SEC(t)                            | 将时间 t 转换为秒                                            | `SELECT TIME_TO_SEC('1:12:00') -> 4320`                      |
| TIMEDIFF(time1, time2)                    | 计算时间差值                                                 | `mysql> SELECT TIMEDIFF("13:10:11", "13:10:10"); -> 00:00:01 mysql> SELECT TIMEDIFF('2000:01:01 00:00:00',    ->                 '2000:01:01 00:00:00.000001');        -> '-00:00:00.000001' mysql> SELECT TIMEDIFF('2008-12-31 23:59:59.000001',    ->                 '2008-12-30 01:01:01.000002');        -> '46:58:57.999999'` |
| TIMESTAMP(expression, interval)           | 单个参数时，函数返回日期或日期时间表达式；有2个参数时，将参数加和 | `mysql> SELECT TIMESTAMP("2017-07-23",  "13:10:11"); -> 2017-07-23 13:10:11 mysql> SELECT TIMESTAMP('2003-12-31');        -> '2003-12-31 00:00:00' mysql> SELECT TIMESTAMP('2003-12-31 12:00:00','12:00:00');        -> '2004-01-01 00:00:00'` |
| TIMESTAMPDIFF(unit,datetime_1,datetime_2) | 计算时间差，返回 datetime_expr2 − datetime_expr1 的时间差    | `mysql> SELECT TIMESTAMPDIFF(DAY,'2003-02-01','2003-05-01');   // 计算两个时间相隔多少天        -> 89 mysql> SELECT TIMESTAMPDIFF(MONTH,'2003-02-01','2003-05-01');   // 计算两个时间相隔多少月        -> 3 mysql> SELECT TIMESTAMPDIFF(YEAR,'2002-05-01','2001-01-01');    // 计算两个时间相隔多少年        -> -1 mysql> SELECT TIMESTAMPDIFF(MINUTE,'2003-02-01','2003-05-01 12:05:55');  // 计算两个时间相隔多少分钟        -> 128885` |
| TO_DAYS(d)                                | 计算日期 d 距离 0000 年 1 月 1 日的天数                      | `SELECT TO_DAYS('0001-01-01 01:01:01') -> 366`               |
| WEEK(d)                                   | 计算日期 d 是本年的第几个星期，范围是 0 到 53                | `SELECT WEEK('2011-11-11 11:11:11') -> 45`                   |
| WEEKDAY(d)                                | 日期 d 是星期几，0 表示星期一，1 表示星期二                  | `SELECT WEEKDAY("2017-06-15"); -> 3`                         |
| WEEKOFYEAR(d)                             | 计算日期 d 是本年的第几个星期，范围是 0 到 53                | `SELECT WEEKOFYEAR('2011-11-11 11:11:11') -> 45`             |
| YEAR(d)                                   | 返回年份                                                     | `SELECT YEAR("2017-06-15"); -> 2017`                         |
| YEARWEEK(date, mode)                      | 返回年份及第几周（0到53），mode 中 0 表示周天，1表示周一，以此类推 | `SELECT YEARWEEK("2017-06-15"); -> 201724`                   |

### 高级函数

| 函数名                                                       | 描述                                                         | 实例                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| BIN(x)                                                       | 返回 x 的二进制编码                                          | 15 的 2 进制编码:`SELECT BIN(15); -- 1111`                   |
| BINARY(s)                                                    | 将字符串 s 转换为二进制字符串                                | `SELECT BINARY "RUNOOB"; -> RUNOOB`                          |
| `CASE expression    WHEN condition1 THEN result1    WHEN condition2 THEN result2   ...    WHEN conditionN THEN resultN    ELSE result END` | CASE 表示函数开始，END 表示函数结束。如果 condition1 成立，则返回 result1, 如果 condition2 成立，则返回 result2，当全部不成立则返回 result，而当有一个成立之后，后面的就不执行了。 | `SELECT CASE  　WHEN 1 > 0 　THEN '1 > 0' 　WHEN 2 > 0 　THEN '2 > 0' 　ELSE '3 > 0' 　END ->1 > 0` |
| CAST(x AS type)                                              | 转换数据类型                                                 | 字符串日期转换为日期：`SELECT CAST("2017-08-29" AS DATE); -> 2017-08-29` |
| COALESCE(expr1, expr2, ...., expr_n)                         | 返回参数中的第一个非空表达式（从左向右）                     | `SELECT COALESCE(NULL, NULL, NULL, 'runoob.com', NULL, 'google.com'); -> runoob.com` |
| CONNECTION_ID()                                              | 返回唯一的连接 ID                                            | `SELECT CONNECTION_ID(); -> 4292835`                         |
| CONV(x,f1,f2)                                                | 返回 f1 进制数变成 f2 进制数                                 | `SELECT CONV(15, 10, 2); -> 1111`                            |
| CONVERT(s USING cs)                                          | 函数将字符串 s 的字符集变成 cs                               | `SELECT CHARSET('ABC') ->utf-8     SELECT CHARSET(CONVERT('ABC' USING gbk)) ->gbk` |
| CURRENT_USER()                                               | 返回当前用户                                                 | `SELECT CURRENT_USER(); -> guest@%`                          |
| DATABASE()                                                   | 返回当前数据库名                                             | `SELECT DATABASE();    -> runoob`                            |
| IF(expr,v1,v2)                                               | 如果表达式 expr 成立，返回结果 v1；否则，返回结果 v2。       | `SELECT IF(1 > 0,'正确','错误')     ->正确`                  |
| [IFNULL(v1,v2)](https://www.runoob.com/mysql/mysql-func-ifnull.html) | 如果 v1 的值不为 NULL，则返回 v1，否则返回 v2。              | `SELECT IFNULL(null,'Hello Word') ->Hello Word`              |
| ISNULL(expression)                                           | 判断表达式是否为 NULL                                        | `SELECT ISNULL(NULL); ->1`                                   |
| LAST_INSERT_ID()                                             | 返回最近生成的 AUTO_INCREMENT 值                             | `SELECT LAST_INSERT_ID(); ->6`                               |
| NULLIF(expr1, expr2)                                         | 比较两个字符串，如果字符串 expr1 与 expr2 相等 返回 NULL，否则返回 expr1 | `SELECT NULLIF(25, 25); ->`                                  |
| SESSION_USER()                                               | 返回当前用户                                                 | `SELECT SESSION_USER(); -> guest@%`                          |
| SYSTEM_USER()                                                | 返回当前用户                                                 | `SELECT SYSTEM_USER(); -> guest@%`                           |
| USER()                                                       | 返回当前用户                                                 | `SELECT USER(); -> guest@%`                                  |
| VERSION()                                                    | 返回数据库的版本号                                           | `SELECT VERSION() -> 5.6.34`                                 |

## 常见问题解决

### 数据无法插入中文

> 修改表字符集为utf8

```sql
alter table account convert to character set utf8;
```



# Spring框架

## 🔸收获

- **基于SpringBoot实现基础SSM框架整合**
- **掌握第三方技术与SpringBoot整合思想**             

## 🔸优点

- **简化开发，降低企业级开发的复杂性**
- **框架整合，高效整合其他技术，提高企业级应用开发与运行效率**

## 🔸核心概念

- **IoC（Inversion of Control）控制反转**

> **使用对象时，由主动new产生对象转换为由外部提供对象，此过程中对象创建控制权由程序转移到外部，此思想称为控制反转**

- **Bean**

> **Ioc容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在Ioc容器中统称为Bean**

## 🔸Spring的发展

------



![image-20220611161625413](PictureFile/【java】入坑笔记.assets/image-20220611161625413.png)

## 🔸Spring Framework系统架构图

------

![image-20220611162500026](PictureFile/【java】入坑笔记.assets/image-20220611162500026.png)

## 🔸XML配置文件开发

### 🔹IoC入门案例

- **导入Spring依赖**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.19</version>
</dependency>
```

- **创建Spring配置文件 名称一般为applicationContext.xml**

```xml
<bean id="bookDao" class="dao.BookDaoImpl"></bean>
```

- **初始化Ioc容器**

```java
//获取ioc容器
ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
//获得bean
BookDao bookDao = (BookDao) ctx.getBean("bookDao");
//执行
bookDao.save();
```

### 🔹DI入门案例

- **删除使用new的形式创建对象的代码**
- **在service层提供setter方法**

```java
public void setBookDao(BookDao bookDao) {
    this.bookDao = bookDao;
}
```

- **绑定service与dao之间的关系**

```xml
<bean id="bookDao" class="dao.BookDaoImpl"/>
    <bean id="bookService" class="service.BookServiceImpl">
<!--第一个bookDao指代变量名称  第二个bookDao指代bean的id-->
        <property name="bookDao" ref="bookDao"></property>
    </bean>
```

### 🔹bean的各种属性关键字解释

- **name（起别名）**

> **service2和service3都与bookService等同**

```xml
<bean id="bookService" name="service2 service3" class="service.BookServiceImpl">
<!--第一个bookDao指代变量名称  第二个bookDao指代bean的id-->
        <property name="bookDao" ref="bookDao"></property>
    </bean>
```

- **scope（设置单例or非单例）**

> 单例

```xml
<bean id="bookDao" class="dao.BookDaoImpl" scope="singleton"/>
```

> 非单例

```xml
<bean id="bookDao" class="dao.BookDaoImpl" scope="prototype"/>
```

- **其他**

> [(46条消息) bean标签的常用属性_lzgsea的博客-CSDN博客_bean标签](https://blog.csdn.net/lzgsea/article/details/79795290)

**适合交给容器进行管理的bean**

> *表现层对象*
>
> *业务层对象*
>
> *数据层对象*
>
> *工具对象*

**不适合交给容器进行管理的bean**

> *封装实体的域对象*

### 🔹bean的实例化

- **构造方法**

  > 提供可访问的构造方法，若无参构造方法如果不存在，将抛出异常BeanCreationException

  - 配置

  ```xml
  <bean id="bookDao" class="dao.BookDaoImpl"/>
  ```

- **静态工厂**

  - 配置

  ```xml
  <bean id="orderDao" class="skyblog.factory.OrderDaoFactory" factory-method="getorderDao/>
  ```

- **实例工厂**

  - 配置

  ```xml
  <bean id="userFactory" class="skyblog.factory.UserDaoFactory"/>
  <bean id="userDao" factory-method="getuserDao" factory-bean="userFactory"/>
  ```

- **FactoryBean**

  - **配置**

  ```xml
  <bean id="bookDao" class="factory.bookDaoFactoryBean"></bean>
  ```

  - **工厂代码部分**

  ```java
  public class bookDaoFactoryBean implements FactoryBean<BookDao> {//泛型填要创建的bean
  
      @Override
      public BookDao getObject() throws Exception {
          return new BookDaoImpl();
      }
  
      @Override
      public Class<?> getObjectType() {
          return BookDao.class;
      }
  }
  ```

### 🔹bean的生命周期

### 🔹生命周期流程

- **初始化容器**

  - **1.创建对象（内存分配）**

  - **2.执行构造方法**


  - **3.执行属性注入（set操作）**

  - **4.执行bean初始化方法**

- **使用bean**

> 1.执行业务操作

- **关闭/销毁容器**

> 1.执行bean销毁方法

### 🔹生命周期的控制

#### ◼方法一(实现接口法)

- **主函数代码**

```java
public static void main(String[] args) {
        //获取ioc容器
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //设置关闭钩子
        ctx.registerShutdownHook();
        //获得bean
//        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        BookService bookService = (BookService) ctx.getBean("bookService");
        //执行
        bookService.save();
    }
```

- **配置**

```xml
<bean id="bookDao" class="dao.BookDaoImpl"></bean>
<bean id="bookService" class="service.BookServiceImpl">
    <property name="bookDao" ref="bookDao"></property>
</bean>
```

- **Service层**

```java
public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {//实现两个接口
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    @Override
    public void save() {
        System.out.println("service...");
        bookDao.save();
    }


    @Override
    public void destroy() throws Exception {
        System.out.println("bean销毁ing");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("bean初始化ing");
    }
}
```

- **Dao层代码实现**

```java
public class BookDaoImpl implements BookDao{
    @Override
    public void save() {
        System.out.println("Dao...");
    }
}
```

#### ◼方法二(配置法)

> init-method
> destroy-method

### 🔹bean的获取

#### ◼使用bean名称获取

```java
BookDao bookDao = (BookDao) ctx.getBean("bookDao");
```

#### ◼使用bean名称获取指定类型

```java
BookDao bookDao = ctx.getBean("bookDao", BookDao.class);
```

#### ◼使用bean类型获取

> 只能有一个同类型的bean

```
BookDao bookDao = ctx.getBean(BookDao.class);
```

### 🔹依赖注入方式

#### ◼setter注入

- **删除使用new的形式创建对象的代码**

- **在service层提供setter方法**

```java
public void setBookDao(BookDao bookDao) {
  this.bookDao = bookDao;
}
```

- **绑定service与dao之间的关系**

```xml
<bean id="bookDao" class="dao.BookDaoImpl"/>
    <bean id="bookService" class="service.BookServiceImpl">
<!--第一个bookDao指代变量名称  第二个bookDao指代bean的id-->
        <property name="bookDao" ref="bookDao"></property>
    </bean>	
```

#### ◼构造器注入

- **提供一个有参构造器**

```java
public BookServiceImpl(BookDao bookDao) {
    this.bookDao = bookDao;
}
```

- **配置**

```xml
<bean id="bookDao" class="dao.BookDaoImpl"></bean>
<bean id="bookService" class="service.BookServiceImpl">
    <!--第一个bookDao指代构造器形参名称  第二个bookDao指代bean的id-->
    <constructor-arg name="bookDao" ref="bookDao"></constructor-arg>
</bean>
```

> **解决参数重复问题**

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
 <!--index代表参数位置-->
<constructor-arg index="0" value="mysq1"/>
<constructor-arg index="1" value="100"/>
</bean>
```

#### ◼自动装配

> 需要setter方法
>
> 自动装配用于引用类型依赖注入，不能对简单类型进行操作
> 使用按类型装配时（byType）必须保障容器中相同类型的bean唯一，推荐使用
> 使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因变量名与配置耦合，不推荐使用
> 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

```xml
<bean id="bookDao" class="dao.BookDaoImpl"></bean>
<bean id="bookService" class="service.BookServiceImpl" autowire="byType"></bean>
```

#### ◼各种集合注入

```xml
<bean id="bookDao"class="dao.bookDao">
	<property name="array">
        <array>
            <value>100</value>
            <value>200</value>
            <value>300</value>
        </array>
    </property>
    <property name="list">
        <list>
            <value>sky</value>
            <value>666</value>
            <value>300</value>
        </list>
    </property>
    <property name="map">
        <map>
            <entry key="country" value="china"/>
            <entry key="province" value="henan"/>
            <entry key="city" value="kaifeng"/>
        </map>
    </property>
    <property name="properties">
        <props>
            <prop key="country">china</prop>
            <prop key="province">henan</prop>
            <prop key="city">kaifengk</prop>
        </props>
    </property>
</bean>
```

- **依赖注入器的选择**

> 1.强制依赖使用构造器进行，使用setteri注入有概率不进行注入导致null对象出现
> 2.可选依赖使用setter注入进行，灵活性强
> 3.Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
> 4.如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用setter注入完成可选依赖的注入
> 5.实际开发过程中还要根据实际情况分析，如果受控对象没有提供setter方法就必须使用构造器注入
> 6.自己开发的模块推荐使用setter注入

## 🔸注解开发

### 🔹基本注解开发【有配置文件形式】

- **关键词**
  - **@Controller:用于表现层bean定义**
  - **@Service:用于业务层bean定义**
  - **@Repository:用于数据层bean定义**

- **配置**

```xml
<context:component-scan base-package="dao"></context:component-scan>
```

-  **主函数代码实现**

```java
public static void main(String[] args) {
    //获取ioc容器
    ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
    //设置关闭钩子
    ctx.registerShutdownHook();
    //获得bean bookDao这个名字要和注解对应
    BookDao bookDao = ctx.getBean("bookDao", BookDao.class);
    //执行
    bookDao.save();
}
```

- **数据层代码实现**

```java
@Repository("bookDao")//名字要和注解对应
public class BookDaoImpl implements BookDao{
    @Override
    public void save() {
        System.out.println("Dao...");
    }
}
```

### 🔹高阶注解开发【无配置文件形式】

- **写配置类**

```java
@Configuration//相当于配置文件中的默认内容
@ComponentScan("dao")//相当于扫描器代码
public class SpringConfig {
}
```

- **主函数代码实现**

```java
public static void main(String[] args) {
    //注解开发加载配置类
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
    BookDao bookDao = ctx.getBean("bookDao", BookDao.class);
    bookDao.save();
}
```

- **数据层代码实现**

```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao{
    @Override
    public void save() {
        System.out.println("Dao...");
    }
}
```

- **控制生命周期代码实现**

```java
@Repository("bookDao2")
public class BookDaoImpl implements BookDao{
    @Override
    public void save() {
        System.out.println("Dao...");
    }

    @PreDestroy
    public void destroy() throws Exception {
        System.out.println("bean销毁ing");
    }

    @PostConstruct
    public void afterPropertiesSet() throws Exception {
        System.out.println("bean初始化ing");
    }
}
```

### 🔹注解依赖注入

- **主函数实现代码**

```java
public static void main(String[] args) {
        //注解开发加载配置类
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        ctx.registerShutdownHook();
        BookService bookService = ctx.getBean("bookService", BookService.class);
        bookService.save();
    }
```

- **Service层代码实现**

```java
@Service("bookService")
public class BookServiceImpl implements BookService {
    @Autowired//自动装配，默认按类型
    @Qualifier("bookDao2")//自动装配按名称
    private BookDao bookDao;

    @Override
    public void save() {
        System.out.println("service...");
        bookDao.save();
    }
}
```

- **Dao层代码实现**

```java
@Repository("bookDao2")//名字要与service层对应
public class BookDaoImpl implements BookDao{
    @Override
    public void save() {
        System.out.println("Dao...");
    }

    @PreDestroy
    public void destroy() throws Exception {
        System.out.println("bean销毁ing");
    }

    @PostConstruct
    public void afterPropertiesSet() throws Exception {
        System.out.println("bean初始化ing");
    }
}
```

- **其他**

> 使用@Value可以实现简单类型注入 

```java
@Value("sky")
private String string; 
```

> 使用@PropertySource("xxx.properties")实现配置注入

```java
@Value("${name}")
private String string;
```

###  🔹注解开发管理第三方bean

> 以注解开发Druid第三方bean为例

- **导入druid包**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```

- **SpringConfig配置**

```java
@Configuration
@ComponentScan({"service","dao","aop"})
@Import(jdbcConfig.class)//导入相应的类，方便spring识别
public class SpringConfig {
}
```

- **jdbcConfig类**

```java
public class jdbcConfig {
    @Bean("dataSource")//声明为bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/db1");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

- **主函数代码实现**

```java
public class App {
    public static void main(String[] args) {
        //注解开发加载配置类
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        DataSource dataSource = ctx.getBean("dataSource", DataSource.class);
        System.out.println(dataSource);
    }
}
```

### 🔹第三方bean依赖注入方法

- **简单类型**

```java
public class jdbcConfig {
    @Value("com.mysql.jdbc.Driver")
    private String driver;
    @Value("jdbc:mysql://localhost:3306/db1")
    private String url;
    @Value("root")
    private String userName;
    @Value("root")
    private String passWord;
    
    @Bean("dataSource")
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName(driver);
        druidDataSource.setUrl(url);
        druidDataSource.setUsername(userName);
        druidDataSource.setPassword(passWord);
        return druidDataSource;
    }
}
```

- **引用类型**

> 引用类型注入直接在方法参数里面写要注入的形参，spring会自动按类型注入

```java
public class jdbcConfig {
    @Value("com.mysql.jdbc.Driver")
    private String driver;
    @Value("jdbc:mysql://localhost:3306/db1")
    private String url;
    @Value("root")
    private String userName;
    @Value("root")
    private String passWord;

    @Bean("dataSource")
    public DataSource dataSource(BookDaoImpl bookDao){//引用数据类型注入直接写要注入的类型
        System.out.println(bookDao);
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName(driver);
        druidDataSource.setUrl(url);
        druidDataSource.setUsername(userName);
        druidDataSource.setPassword(passWord);
        return druidDataSource;
    }
}
```

## 🔸Spring整合Mybatis

[黑马程序员2022最新SSM框架教程_Spring+SpringMVC+Maven高级+SpringBoot+MyBatisPlus企业实用开发技术_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Fi4y1S7ix?p=29&vd_source=d6945470e58840da44f64f5dc572e207)

## 🔸AOP面向切片

### 🔹概念

> AOP（Apect Oriented Programming）面向切面编程，一种编程范式，指导开发者如何组织程序结构

### 🔹本质

> 代理模式

### 🔹核心概念

- **连接点（Join Point）**：程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等

> 往SpringAOP中，理解为方法的执行。

- **切入点（Point cut）**：匹配连接点的式子

> 在SpringAOP中，一个切入点可以只描述一个具体方法，也可以匹配多个方法
>
> 一个具体方法：com.itheima.dao包下的BookDao接口中的无形参无返回值的save方法
>
> 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所有带有一个参数的方法

- **通知（Advice）**：在切入点处执行的操作，也就是共性功能

> 在SpringAOP中，功能最终以方法的形式呈现

- **通知类**：定义通知的类

- **切面（Aspect）**：描述通知与切入点的对应关系

### 🔹示意图

------

![image-20220616222331156](PictureFile/【java】入坑笔记.assets/image-20220616222331156.png)

###  🔹AOP入门案例

-  **导入依赖**

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.9.1</version>
</dependency>
```

- **修改SpingConfig类**

```java
@Configuration//基本参数
@ComponentScan({"service","dao","aop"})//扫描这几个包里面的bean
@EnableAspectJAutoProxy//注解开发aop
public class SpringConfig {
}
```

- **写AOP核心类**

```java
@Component//告诉spring这是个bean，受spring控制
@Aspect//提醒spring这是个aop
public class MyAop {
    //设置切入点 后面写返回值和方法
    @Pointcut("execution(void dao.BookDaoImpl.update())")
    private void pt(){}
	//在前面插入该代码的意思
    @Before("pt()")
    public void method(){
        System.out.println(new Date());
    }
}
```

### 🔹AOP切入点表达式

------

![image-20220622204318171](PictureFile/【java】入坑笔记.assets/image-20220622204318171.png)

------



![image-20220622204519253](PictureFile/【java】入坑笔记.assets/image-20220622204519253.png)

### 🔹AOP通知类型

- **前置通知**

------

![image-20220622205653548](PictureFile/【java】入坑笔记.assets/image-20220622205653548.png)

- **后置通知**

> 类似前置

- **环绕通知**

------

![image-20220622205750609](PictureFile/【java】入坑笔记.assets/image-20220622205750609.png)

> 注意事项

------

![image-20220622205846564](PictureFile/【java】入坑笔记.assets/image-20220622205846564.png)

### 🔹AOP通知获取参数

------

![image-20220623160922994](PictureFile/【java】入坑笔记.assets/image-20220623160922994.png)

------

![image-20220623160957831](PictureFile/【java】入坑笔记.assets/image-20220623160957831.png)

------

![image-20220623161035529](PictureFile/【java】入坑笔记.assets/image-20220623161035529.png)

## 🔸Spring事物

[黑马程序员2022最新SSM框架教程_Spring+SpringMVC+Maven高级+SpringBoot+MyBatisPlus企业实用开发技术_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Fi4y1S7ix?p=40&vd_source=d6945470e58840da44f64f5dc572e207)

## 🔸SpringMVC

### 🔹入门案例

- **导入依赖**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.21</version>
</dependency>

<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

- **UserController类**

```java
@Controller
public class UserController {

    @RequestMapping("/save")
    @ResponseBody//加上表示响应成json，不加表示响应页面
    public String save(){
        System.out.println("user save ....");
        return "{'flag':'true'}";
    }
}
```

- **新建springMvcConfig类**

```java
@Configuration
@ComponentScan("controller")
public class SpringMvcConfig {
}
```

- **设置ServletConfig配置**

```java
public class ServletContainerslnitConfig extends AbstractDispatcherServletInitializer {
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        //加载springmvc配置
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    @Override
    protected String[] getServletMappings() {
        //设置哪些请求归属springmvc处理
        return new String[]{"/"};
    }

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        //加载spring配置
        return null;
    }
}
```

### 🔹SpringMVC请求

- **导入依赖 用于json的相互转化**

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.3</version>
</dependency>
```

- **接收json数据并且封装到pojo里**

```java
@Controller
public class UserSendMsg {
    @RequestMapping("/save")
    @ResponseBody//加上表示响应成json，不加表示响应页面
    public String save(@RequestBody User user){//重点@RequestBody
        System.out.println(user.getName());
        return "ok";
    }
}
```

```json
//前端传参
{
    "name": "hhh",
    "age": 14
}
```

> 接收日期格式

```java
@Controller
public class UserSendMsg {
    @RequestMapping("/save")
    @ResponseBody//加上表示响应成json，不加表示响应页面
    public String save(Date date,@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") Date date1){
        System.out.println(date);
        System.out.println(date1);
        return "ok";
    }
}
```

*前端传参*

![image-20220708164608560](PictureFile/【java】入坑笔记.assets/image-20220708164608560.png)

### 🔹SpringMVC响应

- **响应页面**

```java
@Controller
public class UserSendMsg {
    @RequestMapping("/toPage")
    public String save(){
        return "index.jsp";
    }
}
```

- **响应文本**

```java
@Controller
public class UserSendMsg {
    @RequestMapping("/toText")
    @ResponseBody//加上表示响应成json，不加表示响应页面
    public String save(){
        return "TextOK";
    }
}
```

- **响应json(POJO转json)**

```java
@Controller
public class UserSendMsg {
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    public User save(){
        User sky = new User("sky", 18);
        return sky;
    }
}
```

> 响应json(集合转json)

> 同理。。。

## 🔸REST风格

- **简介**

![image-20220708213004659](PictureFile/【java】入坑笔记.assets/image-20220708213004659.png)

- **入门案例**

```java
@Controller
public class UserSendMsg {
	//保存
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save(){
        System.out.println("users save ......");
        return "OK";
    }
	//删除
    @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable Integer id){//id从路径里面来
        System.out.println("users delete ......"+id);
        return "OK";
    }
	//修改
    @RequestMapping(value = "/users/{id}",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@PathVariable Integer id){//id从路径里面来
        System.out.println("users update ......"+id);
        return "OK";
    }
    //查询
    @RequestMapping(value = "/users/{id}",method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable Integer id){//id从路径里面来
        System.out.println("users getById ......"+id);
        return "OK";
    }
	//查询
    @RequestMapping(value = "/users",method = RequestMethod.GET)
    @ResponseBody
    public String getAll(){
        System.out.println("users getAll ......");
        return "OK";
    }
}
```

> 入门案例化简

```java
//@Controller
//@ResponseBody
@RestController
@RequestMapping("/users")
public class UserSendMsg {

    @PostMapping
    public String save(){
        System.out.println("users save ......");
        return "OK";
    }

    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){//id从路径里面来
        System.out.println("users delete ......"+id);
        return "OK";
    }

    @PutMapping("/{id}")
    public String update(@PathVariable Integer id){
        System.out.println("users update ......"+id);
        return "OK";
    }

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("users getById ......"+id);
        return "OK";
    }

    @GetMapping
    public String getAll(){
        System.out.println("users getAll ......");
        return "OK";
    }
}
```

## 🔸SSM整合配置

- **基本文件结构**

<img src="PictureFile/【java】入坑笔记.assets/image-20220710150720797.png" alt="image-20220710150720797" style="zoom:50%;" />

- **依赖**

```xml
<dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.21</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.21</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.21</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.10</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.7</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.11</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.29</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

- **jdbc.properties**

```xml
jdbc.driver = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/db1
jdbc.username=root
jdbc.password=123456
```

- **jdbcConfig.java**

```java
import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;

public class jdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName(driver);
        druidDataSource.setUrl(url);
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        return  druidDataSource;
    }
}
```

- **MyBatisConfig.java**

```java
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperScannerConfigurer;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;

public class MyBatisConfig {
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setTypeAliasesPackage("skyblog.javaframe.domain");//类型别名
        return factoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("skyblog.javaframe.dao");
        return msc;
    }
}
```

- **ServletConfig.java**

```java
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.servlet.support.AbstractDispatcherServletInitializer;

public class ServletConfig extends AbstractDispatcherServletInitializer {
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        //加载springmvc配置
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    @Override
    protected String[] getServletMappings() {
        //设置哪些请求归属springmvc处理
        return new String[]{"/"};
    }

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        //加载spring配置
        return null;
    }
}
```

- **SpringConfig.java**

```java
@Configuration
@ComponentScan({"skyblog.javaframe.service"})
@PropertySource("jdbc.properties")
@Import({jdbcConfig.class,MyBatisConfig.class})
public class SpringConfig {
}
```

- **SpringMvcConfig.java**

```java
@Configuration
@ComponentScan("skyblog.javaframe.controller")
@EnableWebMvc
public class SpringMvcConfig {
}
```

- **UserDao**

```java
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;
import skyblog.top.springmvc.domain.User;

import java.util.List;

public interface UserDao {
    @Insert("insert into user values(null,#{username},#{password})")
    public void save(User user);

    @Update("update user set username=#{username},password=#{password} where id=#{id}")
    public void update(User user);

    @Delete("delete from user where id=#{id}")
    public void delete(int id);

    @Select("select * from user where id=#{id}")
    public User getById(int id);

    @Select("select * from user")
    public List<User> getAll();
}
```

- **UserService**

```java
import skyblog.top.springmvc.domain.User;

import java.util.List;

public interface UserService {

    public boolean save(User user);

    public boolean update(User user);

    public boolean delete(int id);

    public User getById(int id);

    public List<User> getAll();
}
```

- **UserServiceImpl**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import skyblog.top.springmvc.dao.UserDao;
import skyblog.top.springmvc.domain.User;
import skyblog.top.springmvc.service.UserService;

import java.util.List;

@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDao userDao;

    @Override
    public boolean save(User user) {
        userDao.save(user);
        return true;
    }

    @Override
    public boolean update(User user) {
        userDao.update(user);
        return true;
    }

    @Override
    public boolean delete(int id) {
        userDao.delete(id);
        return true;
    }

    @Override
    public User getById(int id) {
        return userDao.getById(id);
    }

    @Override
    public List<User> getAll() {
        return userDao.getAll();
    }
}
```

- **UserController.java**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import skyblog.top.springmvc.domain.User;
import skyblog.top.springmvc.service.UserService;

import java.util.List;

@Controller
@ResponseBody
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userservice;

    @PostMapping
    public boolean save(@RequestBody User user) {
        System.out.println("新增被执行......");
        return userservice.save(user);
    }

    @PutMapping
    public boolean update(@RequestBody User user) {
        System.out.println("修改被执行......");
        return userservice.update(user);
    }

    @DeleteMapping("/{id}")
    public boolean delete(@PathVariable int id) {
        System.out.println("删除被执行......");
        return userservice.delete(id);
    }

    @GetMapping("/{id}")
    public User getById(@PathVariable int id) {
        System.out.println("通过id查询被执行......");
        return userservice.getById(id);
    }

    @GetMapping
    public List<User> getAll() {
        System.out.println("查询所有被执行......");
        return userservice.getAll();
    }
}
```

- **UserService测试**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class TestUserService {
    @Autowired
    private UserService userService;

    @Test
    public void testGetById(){
        User byId = userService.getById(1);
        System.out.println(byId);
    }

    @Test
    public void testGetAll(){
        List<User> all = userService.getAll();
        System.out.println(all);
    }
}
```

- **Spring事物**

## 🔸Spring异常处理

### 🔹异常处理方法

> 在controller包下创建一个类

```java
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice//声明一个类作为异常处理器类
public class ProjectExceptionAdvice {
    @ExceptionHandler(Exception.class)//拦截异常
    public String doException(Exception e){
        System.out.println("出异常啦，哈哈哈哈");
        return "ERROR";
    }
}
```

### 🔹异常处理方案

> 地址：https://www.bilibili.com/video/BV1Fi4y1S7ix?p=65&t=474.7

- **异常分类**

<img src="PictureFile/【java】入坑笔记.assets/image-20220710174044638.png" alt="image-20220710174044638" style="zoom: 80%;" />

- **异常方案** 

<img src="PictureFile/【java】入坑笔记.assets/image-20220710174255531.png" alt="image-20220710174255531" style="zoom:80%;" />

## 🔸Spring放行

> **config目录下建一个SpringMvcRelease类**

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;

@Configuration
public class SpringMvcRelease extends WebMvcConfigurationSupport {
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }
}
```

> **SpringMvcConfig**

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
@EnableWebMvc
@ComponentScan({"skyblog.top.springmvc.controller","skyblog.top.springmvc.config"})
public class SpringMvcConfig {
}
```

## 🔸SpringMVC拦截器

### 🔹入门案例

> 在controller目录下新建intercept目录，在其中创建ProJectInterceptor类作为拦截器

- **ProJectInterceptor类**

```java
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class ProJectInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截前的操作......");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("拦截后的操作......");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("完成后的操作......");
    }
}
```

- **SpringMvcRelease配置**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;
import skyblog.top.springmvc.controller.interceptor.ProJectInterceptor;

@Configuration
public class SpringMvcRelease extends WebMvcConfigurationSupport {
    @Autowired
    private ProJectInterceptor jectInterceptor;
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(jectInterceptor).addPathPatterns("/users/*");
    }
}
```

### 🔹拦截器的参数

> 略......

### 🔹拦截器链的配置

> https://www.bilibili.com/video/BV1Fi4y1S7ix?p=74&t=429.5



# SpringBoot知识体系笔记

## 🔸概述

## 🔸入门案例

- **创建SpringBoot工程**

------

<img src="PictureFile/【java】入坑笔记.assets/image-20220718103722515.png" alt="image-20220718103722515" style="zoom: 67%;" />

- **手写Controller层**

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/users")
public class UserController {
    @GetMapping()
    public String getAll(){
        System.out.println("getAll");
        return "OK";
    }
}
```

- **运行Application**

## 🔸SpringBoot配置

### 🔹3种基本配置格式

------

![image-20220718104234297](PictureFile/【java】入坑笔记.assets/image-20220718104234297.png)



###  🔹配置文件加载的优先级

> properties > yml > yaml

###   🔹yml封装数据

https://www.bilibili.com/video/BV15b4y1a7yG?p=25&t=1.3

## 🔸SpringBoot整合第三方技术

### 🔹整合Junit

> Boot已经自动整合 
>
> 测试对应的starter
>
> 测试类使用@SpringBootTest修饰  
>
> PS.@SpringBootTest(引导类.class)

```java
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ApplicationTests {

    @Test
    void getByPage(){
        System.out.println("test......");
    }
```

###  🔹整合Mybatis

- **导入依赖**

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

- **Dao层**

```java
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
import skyblog.domain.User;

@Mapper//@Mapper注解不需要在SpringBoot启动类上配置扫描类
public interface UserDao {
    @Select("select * from users where id = #{id}")
    public User selectById(Long id);
}
```

- **测试**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;

@SpringBootTest()
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        System.out.println("test......");
        System.out.println(userDao.selectById(4L));
    }
}
```

### 🔹整合Mybatis-Plus

- **依赖**

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```

- **Dao层**

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import skyblog.domain.User;

@Mapper
public interface UserDao extends BaseMapper<User> {
}
```

- **测试**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.dao.UserDao;

@SpringBootTest()
class ApplicationTests {
    @Autowired
    private UserDao userDao;

    @Test
    void getByPage(){
        System.out.println(userDao.selectById(4L));
    }
}
```

### 🔹整合Druid数据源

- **依赖**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.11</version>
</dependency>
```

- **配置数据源**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db1
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
```

## 🔸SpringBoot基本开发

### 🔹数据层标准开发

> Mybatis-Plus开发

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import skyblog.domain.User;

@Mapper
public interface UserDao extends BaseMapper<User> {
}
```

### 🔹业务层标准开发

- **接口**

```java
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import skyblog.domain.User;

public interface UserService {
    Boolean save(User user);
    Boolean deleteById(Long id);
    Boolean update(User user);
    User selectById(Long id);
    Page<User> getByPage(int currentPage, int pageSize);
}
```

- **实现类**

```JAVA
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import skyblog.dao.UserDao;
import skyblog.domain.User;
import skyblog.service.UserService;

@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDao userDao;

    @Override
    public Boolean save(User user) {
        return userDao.insert(user) > 0;
    }

    @Override
    public Boolean deleteById(Long id) {
        return userDao.deleteById(id) > 0;
    }

    @Override
    public Boolean update(User user) {
        return userDao.updateById(user) > 0;
    }

    @Override
    public User selectById(Long id) {
        return userDao.selectById(id);
    }

    @Override
    public Page<User> getByPage(int currentPage, int pageSize) {
        Page<User> userPage = new Page<>(currentPage, pageSize);
        return userDao.selectPage(userPage, null);
    }
}
```

- **MP分页拦截器**

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mpi = new MybatisPlusInterceptor();
        mpi.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpi;
    }
}
```

- **测试**


```java
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.domain.User;
import skyblog.service.UserService;

@SpringBootTest()
class ApplicationTests {
    @Autowired
    private UserService userService;

    @Test
    void selectById() {
        System.out.println(userService.selectById(4L));
    }

    @Test
    void getPage() {
        Page<User> page = userService.getByPage(1, 5);
        System.out.println("当前页码：" + page.getCurrent());
        System.out.println("每页显示的数目：" + page.getSize());
        System.out.println("一共有多少页：" + page.getPages());
        System.out.println("一个有多少条数据：" + page.getTotal());
        System.out.println("数据：" + page.getRecords());
    }
}
```

### 🔹业务层快速开发

- **接口**

> 继承IService接口

```java
import com.baomidou.mybatisplus.extension.service.IService;
import skyblog.domain.User;

public interface UserServiceQuick extends IService<User> {
}
```

- **实现类**

> 继承ServiceImpl实现类

```java
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;
import skyblog.dao.UserDao;
import skyblog.domain.User;

@Service
public class UserServiceQuickImpl extends ServiceImpl<UserDao, User> {
}
```

- **测试**

> 记得配置MP拦截器

```java
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import skyblog.domain.User;
import skyblog.service.Impl.UserServiceQuickImpl;

@SpringBootTest()
class ApplicationTests {
    @Autowired
    private UserServiceQuickImpl userServiceQuick;

    @Test
    void selectById() {
        System.out.println(userServiceQuick.getById(4L));
    }

    @Test
    void getPage() {
        Page<User> page = new Page<>(1, 5);
        userServiceQuick.page(page);
        System.out.println("当前页码：" + page.getCurrent());
        System.out.println("每页显示的数目：" + page.getSize());
        System.out.println("一共有多少页：" + page.getPages());
        System.out.println("一个有多少条数据：" + page.getTotal());
        System.out.println("数据：" + page.getRecords());
    }
}
```

### 🔹表现层标准开发

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import skyblog.domain.User;
import skyblog.service.Impl.UserServiceImpl;

@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserServiceImpl userService;
    @GetMapping("/{id}")
    public User getById(@PathVariable Long id){
        return userService.selectById(id);
    }
}
```

### 🔹表现层数据一致性处理

- **创建R类**

```java
public class R {
    private boolean flag;
    private Object data;

    public R() {
    }

    public R(boolean flag, Object data) {
        this.flag = flag;
        this.data = data;
    }

    public R(boolean flag) {
        this.flag = flag;
    }

    public boolean isFlag() {
        return flag;
    }

    public void setFlag(boolean flag) {
        this.flag = flag;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }

    @Override
    public String toString() {
        return "R{" +
                "flag=" + flag +
                ", data=" + data +
                '}';
    }
}
```

- **表现层**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import skyblog.domain.R;
import skyblog.service.Impl.UserServiceImpl;

@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserServiceImpl userService;
    @GetMapping("/{id}")
    public R getById(@PathVariable Long id){
        R r = new R(true,userService.selectById(id));
        return r;
    }
}
```

## 🔸SpringBoot部署

### 🔹Window基本命令

```xml
#查询端口
netstat -ano
#查询指定端口
netstat -ano |findstr [端口号]
#根据进程PID查询进程名称
tasklist |findstr [端口号]
#根据PID杀死任务
taskkill /F /PID [进程PID]
#根据进程名称杀死任务
taskkill -f -t -im [进程名称]
```

# Springboot实战笔记

## 压缩包问题

> ZipUtils类。
>
> 支持压缩成zip文件，解压zip文件。
>
> 支持解压中文压缩包，支持解压中文文件和目录
>
> 支持解压文件带空格和特殊符号文件

```java
package com.example.filehostingplatformdemo.utils;


import org.apache.tools.zip.ZipFile;
import org.apache.tools.zip.ZipOutputStream;

import java.io.*;
import java.util.Enumeration;
import java.util.zip.ZipEntry;

public class ZipUtils {
    /**
     * 使用GBK编码可以避免压缩中文文件名乱码
     */
    private static final String CHINESE_CHARSET = "GBK";
    /**
     * 文件读取缓冲区大小
     */
    private static final int CACHE_SIZE = 1024;

    /**
     * 压缩文件
     *
     * @param sourceFolder 压缩文件夹
     * @param zipFilePath  压缩文件输出路径
     */
    public static void zip(String sourceFolder, String zipFilePath) {
        OutputStream os = null;
        BufferedOutputStream bos = null;
        ZipOutputStream zos = null;
        try {
            os = new FileOutputStream(zipFilePath);
            bos = new BufferedOutputStream(os);
            zos = new ZipOutputStream(bos);
            // 解决中文文件名乱码
            zos.setEncoding(CHINESE_CHARSET);
            File file = new File(sourceFolder);
            String basePath = null;
            //压缩文件夹
            if (file.isDirectory()) {
                basePath = file.getPath();
            } else {
                basePath = file.getParent();
            }
            zipFile(file, basePath, zos);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (zos != null) {
                    zos.closeEntry();
                    zos.close();
                }
                if (bos != null) {
                    bos.close();
                }
                if (os != null) {
                    os.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 递归压缩文件
     *
     * @param parentFile 父文件
     * @param basePath   基础路径
     * @param zos        ZipOutputStream对象
     * @throws Exception
     */
    private static void zipFile(File parentFile, String basePath, ZipOutputStream zos) throws Exception {
        File[] files = new File[0];
        if (parentFile.isDirectory()) {
            files = parentFile.listFiles();
        } else {
            files = new File[1];
            files[0] = parentFile;
        }
        String pathName;
        InputStream is;
        BufferedInputStream bis;
        byte[] cache = new byte[CACHE_SIZE];
        for (File file : files) {
            if (file.isDirectory()) {
                pathName = file.getPath().substring(basePath.length() + 1) + File.separator;
                zos.putNextEntry((org.apache.tools.zip.ZipEntry) new ZipEntry(pathName));
                zipFile(file, basePath, zos);
            } else {
                pathName = file.getPath().substring(basePath.length() + 1);
                is = new FileInputStream(file);
                bis = new BufferedInputStream(is);
                zos.putNextEntry((org.apache.tools.zip.ZipEntry) new ZipEntry(pathName));
                int nRead = 0;
                while ((nRead = bis.read(cache, 0, CACHE_SIZE)) != -1) {
                    zos.write(cache, 0, nRead);
                }
                bis.close();
                is.close();
            }
        }
    }

    /**
     * 解压压缩包
     *
     * @param zipFilePath 压缩文件路径
     * @param destDir     解压目录
     */
    public static void unZip(String zipFilePath, String destDir) {
        destDir += "/";
        ZipFile zipFile = null;
        BufferedInputStream bis = null;
        FileOutputStream fos = null;
        BufferedOutputStream bos = null;
        try {
            zipFile = new ZipFile(zipFilePath, CHINESE_CHARSET);
            Enumeration<org.apache.tools.zip.ZipEntry> zipEntries = zipFile.getEntries();
            File file, parentFile;
            ZipEntry entry;
            byte[] cache = new byte[CACHE_SIZE];
            while (zipEntries.hasMoreElements()) {
                entry = (ZipEntry) zipEntries.nextElement();
                if (entry.isDirectory()) {
                    new File(destDir + entry.getName()).mkdirs();
                    continue;
                }
                bis = new BufferedInputStream(zipFile.getInputStream((org.apache.tools.zip.ZipEntry) entry));
                file = new File(destDir + entry.getName());
                parentFile = file.getParentFile();
                if (parentFile != null && (!parentFile.exists())) {
                    parentFile.mkdirs();
                }
                fos = new FileOutputStream(file);
                bos = new BufferedOutputStream(fos, CACHE_SIZE);
                int readIndex = 0;
                while ((readIndex = bis.read(cache, 0, CACHE_SIZE)) != -1) {
                    fos.write(cache, 0, readIndex);
                }
                bos.flush();
                bos.close();
                fos.close();
                bis.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                zipFile.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        //目标文件夹记得多加个反斜杠
        ZipUtils.unZip("D:\\文件托管平台.zip", "D:\\作业1");
    }
}
```
