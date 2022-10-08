# Go网络编程

## 简单TCP聊天实现

#### 服务端

```go
package main

import (
	"fmt"
	"net"
)

func process(conn net.Conn) {
	//延迟关闭conn
	defer func(conn net.Conn) {
		err := conn.Close()
		if err != nil {
			println(err.Error())
		}
	}(conn)

	//循环接收客户端消息
	for {
		buf := make([]byte, 1024)
		//读取客户端消息，如没有则阻塞等待
		n, err := conn.Read(buf)
		//客户端下线
		if err != nil {
			fmt.Printf("%v 客户端下线\n", conn.RemoteAddr().String())
			return
		}
		//打印接受的信息
		fmt.Printf("%v 发送:%v", conn.RemoteAddr().String(), string(buf[:n]))
	}
}

func main() {
	//开启tcp监听本地8888端口
	listen, err := net.Listen("tcp", "localhost:8888")
	println("开始监听~")
	if err != nil {
		println(err.Error())
		return
	}

	//延迟关闭listen
	defer func(listen net.Listener) {
		err := listen.Close()
		if err != nil {
			println("出错啦")
		}
	}(listen)

	//等待客户端连接
	for {
		//println("等待客户端连接")
		conn, err := listen.Accept()
		if err != nil {
			println(err.Error())
		}
		fmt.Printf("%v上线 \n", conn.RemoteAddr().String())
		//开启消息接收协程
		go process(conn)
	}
}

```

#### 客户端

```go
package main

import (
   "bufio"
   "fmt"
   "net"
   "os"
)

func main() {
   //连接服务端
   conn, err := net.Dial("tcp", "localhost:8888")
   if err != nil {
      println("出错啦", err.Error())
   }

   //延迟关闭conn
   defer func(conn net.Conn) {
      err := conn.Close()
      if err != nil {
         println(err.Error())
      }
   }(conn)
   
   reader := bufio.NewReader(os.Stdin)
   //循环发送信息
   for {
      //读取用户输入以\n结尾
      msg, err := reader.ReadString('\n')
      if err != nil {
         println(err.Error())
      }

      //退出命令
      if msg == "exit" {
         println("退出成功")
         break
      }

      //发送输入的信息给服务端
      n, err := conn.Write([]byte(msg))
      if err != nil {
         println("出错啦~", err.Error())
      }
      fmt.Printf("[发送了%v个字节]\n", n)
   }
}
```

# Gin框架

## 简单请求接口

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
   err := r.Run(":8888")
   if err != nil {
      fmt.Printf("%v", err)
   }
}
```

## 返回json

> 传统方法

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

> 结构体

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

## Html模板渲染

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