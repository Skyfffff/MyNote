# SpringBoot知识体系笔记

## 🔸概述

## 🔸入门案例

- **创建SpringBoot工程**

------

<img src="PictureFile/【SpringBoot】从复杂到简洁.assets/image-20220718103722515.png" alt="image-20220718103722515" style="zoom: 67%;" />

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

![image-20220718104234297](PictureFile/【SpringBoot】从复杂到简洁.assets/image-20220718104234297.png)



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

@Mapper
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

