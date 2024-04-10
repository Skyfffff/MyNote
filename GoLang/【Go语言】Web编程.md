# Gin框架

## 路由

### 基本路由

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
)

func main() {
   r := gin.Default()

   r.GET("/", func(c *gin.Context) {
      c.String(200, "hello world")
   })

   r.GET("/get", func(c *gin.Context) {
      c.String(200, "get请求")
   })

   r.POST("/post", func(context *gin.Context) {
      context.String(200, "post请求")
   })
    //开启8888端口
    // Run("里面不指定端口号默认为8080") 
   err := r.Run(":8888")
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 路由分组

```go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   userGroup := r.Group("/user")
   {
      userGroup.GET("/login", login)
      userGroup.GET("/loginout", loginout)
   }

   adminGroup := r.Group("/admin")
   {
      adminGroup.GET("/login", login)
      adminGroup.GET("/loginout", loginout)
   }
   r.Run()
}

func login(c *gin.Context) {
   c.String(http.StatusOK, "login")
}

func loginout(c *gin.Context) {
   c.String(http.StatusOK, "loginout")
}
```

### 路由分组抽离

目录结构                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  

```
GoWeb
  │  go.mod
  │  go.sum
  │  main.go
  │
  ├─routers
  │      adminRouters.go
  │      apiRouters.go

```

main.go

```go
func main() {
   router := gin.Default()

   routers.AdminRoutersInit(router)
   routers.ApiRoutersInit(router)

   router.Run(":8080")
}
```

adminRouters.go

```go
package routers

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func AdminRoutersInit(r *gin.Engine) {
   defaultGroup := r.Group("/admin")
   {
      defaultGroup.GET("/login", login)
      defaultGroup.GET("/add", addUser)
      defaultGroup.GET("/delete", deleteUser)
   }
}

func login(c *gin.Context) {
   c.String(http.StatusOK, "登录成功")
}

func addUser(c *gin.Context) {
   c.String(http.StatusOK, "成功添加用户")
}

func deleteUser(c *gin.Context) {
   c.String(http.StatusOK, "成功删除用户")
}
```

apiRouters.go

```go
package routers

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func ApiRoutersInit(r *gin.Engine) {
   defaultGroup := r.Group("/api")
   {
      defaultGroup.GET("/pay", pay)
      defaultGroup.GET("/send", send)
      defaultGroup.GET("/get", get)
   }
}

func pay(c *gin.Context) {
   c.String(http.StatusOK, "我是支付接口")
}

func send(c *gin.Context) {
   c.String(http.StatusOK, "我是发送接口")
}

func get(c *gin.Context) {
   c.String(http.StatusOK, "我是获取接口")
}
```

## 返回数据

### Json

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
)

func main() {
   r := gin.Default()

   r.GET("/json", func(c *gin.Context) {
      c.JSON(200, map[string]interface{}{
         "success": true,
         "msg":     "测试成功！",
      })
   })
   
   //代码简写
   //r.GET("/json", func(c *gin.Context) {
   // c.JSON(200, gin.H{
   //    "success": true,
   //    "msg":     "测试成功！",
   // })
   //})

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 结构体

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

type User struct {
   Code int    `json:"code"`
   Id   int    `json:"id"`
   Name string `json:"name"`
   Msg  string `json:"msg"`
}

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      user1 := &User{
         Code: 200,
         Id:   1001,
         Name: "sky",
         Msg:  "admin",
      }
      c.JSON(http.StatusOK, gin.H{
         "title": "我是新闻",
         "user":  user1,
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 重定向

```go
package main

import (
   "net/http"

   "github.com/gin-gonic/gin"
)

func main() {
   r := gin.Default()
   r.GET("/index", func(c *gin.Context) {
      c.Redirect(http.StatusMovedPermanently, "http://skyblog.top")
   })
   r.Run()
}
```

## Gin渲染

### 入门案例

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", gin.H{
         "msg": "Hello World",
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 模板变量

```
${{value}}
```

> 前端模板

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hello world</title>
</head>
<body>
    {{$value := .msg}}
    <p>{{$value}}</p>
</body>
</html>
```

> 后端代码

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", gin.H{
         "msg": "sky666",
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 条件判断

```
{{if ge .score 60}}
xxxx
{{else}}
xxxx
{{end}}
```

### 比较函数

| 函数 | 解释                   |
| ---- | :--------------------- |
| eq   | 如果arg1 == arg2返回真 |
| ne   | 如果arg1 !=arg2返回真  |
| lt   | 如果arg1 < arg2返回真  |
| le   | 如果arg1 <= arg2返回真 |
| gt   | 如果arg1 > arg2返回真  |
| ge   | 如果arg1 >= arg2返回真 |

#### 案例

> 前端页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hello world</title>
</head>
<body>
    <div>
        {{if ge .score 60}}
            <p>及格啦,分数是{{.score}}分</p>
        {{else}}
            <p>不及格,分数是{{.score}}分</p>
        {{end}}
    </div>
</body>
</html>
```

> 后端页面

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", gin.H{
         "score": 61,
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 遍历函数

```
{{range $value := .city}}
	<p>{{$value}}</p>
{{end}}
```

#### 切片遍历案例

> 前端代码

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>hello world</title>
    </head>
    <body>
        <div>
            {{range $value := .city}}
            <p>{{$value}}</p>
            {{end}}
        </div>
    </body>
</html>
```

> 后端代码

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", gin.H{
         "city": []string{"上海", "北京", "广州", "深圳"},
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

#### 切片结构体遍历

> 前端

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>hello world</title>
    </head>
    <body>
        <div>
            {{ range $user := .user}}
                <p>{{$user.Id}}</p>
                <p>{{$user.Name}}</p>
                <p>{{$user.Msg}}</p>
            {{end}}
        </div>
    </body>
</html>
```

> 后端

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

type User struct {
   Id   int    `json:"id,omitempty"`
   Name string `json:"name,omitempty"`
   Msg  string `json:"msg,omitempty"`
}

func main() {
   r := gin.Default()
   //设置模板目录
   r.LoadHTMLGlob("templates/*")

   r.GET("/html", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", gin.H{
         "user": []interface{}{
            &User{
               Id:   1,
               Name: "Sky",
               Msg:  "你干嘛~哎呦~~",
            },
            &User{
               Id:   2,
               Name: "吴博",
               Msg:  "咖喱咖喱~",
            },
            &User{
               Id:   3,
               Name: "苗恩峰",
               Msg:  "你干嘛~哎呦~~",
            },
         },
      })
   })

   err := r.Run()
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

### 自定义模板函数

xxx

### 模板的嵌套

xxx

## 静态资源释放

xxx

## 获取前端数据

### Get和Post参数获取

> 前端

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>hello world</title>
    </head>
    <body>
        <form action="/post" method="post">
            username: <input type="text" name="username"/> <br>
            password: <input type="text" name="password"/> <br>
            <button type="submit">登录</button>
        </form>
        <p>msg:{{.msg}}</p>
        <p>Love:{{.love}}</p>
        <p>username:{{.username}}</p>
        <p>password:{{.password}}</p>
    </body>
</html>
```

> 后端

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()

	r.LoadHTMLGlob("./templates/*")

	r.GET("/get", func(c *gin.Context) {
		msg := c.Query("msg")
		love := c.DefaultQuery("love", "Yes")
		c.HTML(http.StatusOK, "hello.html", gin.H{
			"msg":  msg,
			"love": love,
		})
	})

	r.POST("/post", func(c *gin.Context) {
		username := c.PostForm("username")
		password := c.PostForm("password")
		
		c.HTML(http.StatusOK, "hello.html", gin.H{
			"username": username,
			"password": password,
		})
	})

	err := r.Run()
	if err != nil {
		fmt.Printf("%v", err)
	}
}
```

### 参数绑定

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

type Userinfo struct {
   //json:格式输出  form:格式输入
   Username string `json:"username" form:"username"`
   Password string `json:"password" form:"password"`
}

func main() {
   r := gin.Default()
   r.GET("/user", func(c *gin.Context) {
      var u Userinfo
      err := c.ShouldBind(&u)
      if err != nil {
         c.JSON(http.StatusBadGateway, gin.H{
            "error": err.Error(),
         })
      } else {
         c.JSON(http.StatusOK, u)
      }
      fmt.Printf("%#v\n", &u)
   })
   r.Run()
}
```

## 文件上传

### 上传单个文件

> 前端代码

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <form action="/upload" method="post" enctype="multipart/form-data">
            上传文件:<input type="file" name="f1">
            <input type="submit" value="提交">
        </form>
    </body>
</html>
```

> 后端

```go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   r.LoadHTMLGlob("./templates/*")
   
   r.GET("/", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", nil)
   })

   r.POST("/upload", func(c *gin.Context) {
      file, err := c.FormFile("f1")
      if err != nil {
         c.String(http.StatusInternalServerError, "上传出错")
      }
      //保存在指定目录
      c.SaveUploadedFile(file, file.Filename)
      c.JSON(http.StatusOK, gin.H{
         "msg": file.Filename,
      })
   })

   r.Run()
}
```

### 上传多个文件

> 前端

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <form action="upload" method="post" enctype="multipart/form-data">
            上传文件:<input type="file" name="files" multiple>
            <input type="submit" value="提交">
        </form>
    </body>
</html>
```

> 后端

```go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   r.LoadHTMLGlob("./templates/*")
   r.GET("/", func(c *gin.Context) {
      c.HTML(http.StatusOK, "hello.html", nil)
   })
	//设置最大文件大小 20mb
   r.MaxMultipartMemory = 10 << 20

   r.POST("/upload", func(c *gin.Context) {
      //获取多部分表单数据
      form, err := c.MultipartForm()
      if err != nil {
         c.String(http.StatusInternalServerError, "上传出错啦")
      }
		//获取所有文件
      files := form.File["files"]
		//逐个保存
      for _, file := range files {
         if err := c.SaveUploadedFile(file, file.Filename); err != nil {
            c.String(http.StatusInternalServerError, "保存出错啦")
            return
         }
      }
      c.String(http.StatusOK, "保存成功")                                                                                  
   })                                                                                                       

   r.Run()
}
```

## 会话操作

### Cookie

```go
package main

import (
   "fmt"
   "github.com/gin-gonic/gin"
   "net/http"
)

func main() {
   r := gin.Default()
   
   r.GET("/cookie", func(c *gin.Context) {
      //获取客户端的cookie 没找到就err
      cookie, err := c.Cookie("name")

      if err != nil {
         fmt.Printf("%v", err)
          //  maxAge int, 单位为秒
         // path,cookie所在目录
         // domain string,域名
         //   secure 是否智能通过https访问
         // httpOnly bool  是否允许别人通过js获取自己的cookie
         c.SetCookie("name", "nil", 600, "/", "localhost", false, true)
      }

      c.String(http.StatusOK, cookie)
   })
   
   //设置cookie
   r.GET("/setcookie", func(c *gin.Context) {
      name := c.Query("name")
      //设置cookie
       //  maxAge int, 单位为秒
         // path,cookie所在目录
         // domain string,域名
         //   secure 是否智能通过https访问
         // httpOnly bool  是否允许别人通过js获取自己的cookie
      c.SetCookie("name", name, 600, "/", "localhost", false, true)
      c.String(http.StatusOK, name)
   })
   
   r.Run()
}
```



# Go操作Mysql

## Mysql事物

```go
package main

import (
   "fmt"
   _ "github.com/go-sql-driver/mysql"
   "github.com/jmoiron/sqlx"
)

var Db *sqlx.DB

func init() {
   //连接mysql
   database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/db1")
   if err != nil {
      fmt.Println("open mysql failed,", err)
      return
   }
   Db = database
}

func main() {
   //开启事物
   conn, err2 := Db.Begin()
   if err2 != nil {
      fmt.Printf("%v", err2)
      return
   }

   //sql语句
   r, err := conn.Exec("insert into users(username, password)values(?,?)", "ShiWuTest", "ShiWuTest")
   if err != nil {
      fmt.Println("exec failed, ", err)
      return
   }

   id, err := r.LastInsertId()
   if err != nil {
      fmt.Println("exec failed, ", err)
      //回滚事物
      conn.Rollback()
      return
   }

   conn.Commit()
   fmt.Println("insert succ:", id)
}
```

## 基本操作

### 增

```go
package main

import (
   "fmt"
   _ "github.com/go-sql-driver/mysql"
   "github.com/jmoiron/sqlx"
)

var Db *sqlx.DB

func init() {
   //连接mysql
   database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/db1")
   if err != nil {
      fmt.Println("open mysql failed,", err)
      return
   }
   Db = database
}

func main() {
   //sql语句
   r, err := Db.Exec("insert into users(username, password)values(?,?)", "Skyfff", "123456")
   if err != nil {
      fmt.Println("exec failed, ", err)
      return
   }
   
   id, err := r.LastInsertId()
   if err != nil {
      fmt.Println("exec failed, ", err)
      return
   }

   fmt.Println("insert succ:", id)
}
```

### 删

```go
package main

import (
   "fmt"
   _ "github.com/go-sql-driver/mysql"
   "github.com/jmoiron/sqlx"
)

var Db *sqlx.DB

func init() {
   //连接mysql
   database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/db1")
   if err != nil {
      fmt.Println("open mysql failed,", err)
      return
   }
   Db = database
}

func main() {
   //sql语句
   res, err := Db.Exec("delete from users where id = ?", 666)
   if err != nil {
      fmt.Printf("%v", err)
      return
   }

   aff, err := res.RowsAffected()
   if err != nil {
      fmt.Printf("%v", err)
      return
   }

   fmt.Printf("%v", aff)
}
```

### 改

```go
package main

import (
   "fmt"
   _ "github.com/go-sql-driver/mysql"
   "github.com/jmoiron/sqlx"
)

var Db *sqlx.DB

func init() {
   //连接mysql
   database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/db1")
   if err != nil {
      fmt.Println("open mysql failed,", err)
      return
   }
   Db = database
}

func main() {
   //sql语句
   res, err := Db.Exec("update users set username=? where id = ?", "skyUpdate", 4)
   if err != nil {
      fmt.Printf("%v", err)
      return
   }

   aff, err := res.RowsAffected()
   if err != nil {
      fmt.Printf("%v", err)
      return
   }

   fmt.Printf("%v", aff)
}
```

### 查

```go
package main

import (
   "fmt"
   _ "github.com/go-sql-driver/mysql"
   "github.com/jmoiron/sqlx"
)

type Users struct {
   Id       int64  `db:"id"`
   Username string `db:"username"`
   Password string `db:"password"`
   Deleted  int    `db:"deleted"`
   Version  int    `db:"version"`
}

var Db *sqlx.DB

func init() {
   //连接mysql
   database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/db1")
   if err != nil {
      fmt.Println("open mysql failed,", err)
      return
   }
   Db = database
}

func main() {
   //切片类型
   var user []Users
   //sql语句
   err := Db.Select(&user, "select * from users")
   if err != nil {
      fmt.Printf("%v", err)
      return
   }

   fmt.Printf("%v", user)
}
```

