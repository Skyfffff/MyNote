# SpringBoot知识体系笔记

## 概述

## 入门案例

- ### 创建SpringBoot工程

------

<img src="PictureFile/【SpringBoot】从复杂到简洁.assets/image-20220718103722515.png" alt="image-20220718103722515" style="zoom: 67%;" />

- ### 手写Controller层

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

- ### 运行Application

## SpringBoot配置格式

------

![image-20220718104234297](PictureFile/【SpringBoot】从复杂到简洁.assets/image-20220718104234297.png)

