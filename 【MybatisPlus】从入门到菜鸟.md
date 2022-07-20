# MyBatisPlus从入门到实践

## 简介

[MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis (opens new window)](https://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**官网**：[MyBatis-Plus (baomidou.com)](https://baomidou.com/)

## 特点

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

## 框架结构

![framework](【MybatisPlus】从入门到菜鸟.assets/mybatis-plus-framework.jpg)

## 入门案例

- ### SpringBoot整合Mybatis-plus

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

- ### 配置yml数据源信息

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db1
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource
```

- ### Dao层

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import skyblog.domain.User;

@Mapper
public interface UserDao extends BaseMapper<User> { //自动生成sql无需手写
}
```

- ### 测试

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

##   查询

### 分页查询

- #### 添加Page拦截器

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

- #### 测试

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

- #### 输出

```xml
当前页码：1
每页显示的数目：2
一共有多少页：11
一个有多少条数据：22
数据：[User{id=1, username='UpdateTest', password='123456'}, User{id=4, username='people2', password='123'}]
```

### 条件查询

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
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
        //方式一：普通格式
//        QueryWrapper<User> qw = new QueryWrapper<>();
//        qw.lt("id",10);//查询id小于10的
//        System.out.println(userDao.selectList(qw));

        //方式二：Lambda格式
//        QueryWrapper<User> qw = new QueryWrapper<>();
//        qw.lambda().lt(User::getId,10);
//        System.out.println(userDao.selectList(qw));
        
        //方式三：直接用Lambda条件查询类
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
        lqw.lt(User::getId,10);
        System.out.println(userDao.selectList(lqw));
    }
}
```
