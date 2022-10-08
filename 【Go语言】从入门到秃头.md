#  Go语言基础

## Go基本命令

```
go env用于打印Go语言的环境信息。

go run命令可以编译并运行命令源码文件。

go get可以根据要求和实际情况从互联网上下载或更新指定的代码包及其依赖包，并对它们进行编译和安装。

go build命令用于编译我们指定的源码文件或代码包以及它们的依赖包。

go install用于编译并安装指定的代码包及它们的依赖包。

go clean命令会删除掉执行其它命令时产生的一些文件和目录。

go doc命令可以打印附于Go语言程序实体上的文档。我们可以通过把程序实体的标识符作为该命令的参数来达到查看其文档的目的。

go test命令用于对Go语言编写的程序进行测试。

go list命令的作用是列出指定的代码包的信息。

go fix会把指定代码包的所有Go语言源码文件中的旧版本代码修正为新版本的代码。

go vet是一个用于检查Go语言源码中静态错误的简单工具。

go tool pprof命令来交互式的访问概要文件的内容。go env用于打印Go语言的环境信息。

go run命令可以编译并运行命令源码文件。

go get可以根据要求和实际情况从互联网上下载或更新指定的代码包及其依赖包，并对它们进行编译和安装。

go build命令用于编译我们指定的源码文件或代码包以及它们的依赖包。

go install用于编译并安装指定的代码包及它们的依赖包。

go clean命令会删除掉执行其它命令时产生的一些文件和目录。

go doc命令可以打印附于Go语言程序实体上的文档。我们可以通过把程序实体的标识符作为该命令的参数来达到查看其文档的目的。

go test命令用于对Go语言编写的程序进行测试。

go list命令的作用是列出指定的代码包的信息。

go fix会把指定代码包的所有Go语言源码文件中的旧版本代码修正为新版本的代码。

go vet是一个用于检查Go语言源码中静态错误的简单工具。

go tool pprof命令来交互式的访问概要文件的内容。
```

## 变量的声明

> 单变量声明

```go
package main

func main() {
    
   //变量声明方式一
   var name1 string = "Sky"
   println(name1)
    
   //变量声明方式二
   var name2 = "Sky"
   println(name2)
    
   //变量声明方式三(常用)
   name3 := "Sky"
   println(name3)
}
```

> 多变量声明

```go
package main

func main() {

   //多变量声明方式一
   var name1, name2, name3 string = "sky1", "sky2", "sky3"
   println(name1, name2, name3)

   //多变量声明方式二
   var name4, name5, name6 = "sky1", "sky2", "sky3"
   println(name4, name5, name6)

   //多变量声明方式三(常用)
   name7, name8, name9 := "sky1", "sky2", "sky3"
   println(name7, name8, name9)
}
```

## 常量的声明

```go
package main

func main() {

   //枚举
   const (
      age   = 12
      name  = "sky"
      phone = 10086
   )

   //枚举iota
   const (
      a = iota   //0
      b          //1
      c          //2
      d = "sky"  //独立值 sky
      e = "haha" //独立值 haha
      f          //haha
      g = iota   //6
      h          //7
   )

   //0 1 2 sky haha haha 6 7
   println(a, b, c, d, e, f, g, h)

   //常量
   const WIDTH int = 200
   const HEIGHT int = 300
   println(WIDTH * HEIGHT)

   //多个常量
   const A, B, C = "sky", 123, true
   println(A, B, C)

}
```

## 条件语句

> if语句

```go
package main

func main() {
   a := 10
   if a >= 10 {
      println("大于等于10")
   } else {
      println("小于10")
   }
}
```

> switch语句

```go
package main

func main() {
   i := 1
   switch i {
   case 1:
      println(i)
   case 2:
      println(i)
   case 3:
      println(i)
   default:
      println("defalut")
   }
}
```

## 循环语句

> for语句

```go
package main

func main() {
	i := 0
	for i = 0; i <= 10; i++ {
		println(i)
	}
}
```

## 函数

```go
package main

func main() {
   println(max(123, 55))
   println(swap("sky", "666"))
}

// 交换ab的值
func swap(a, b string) (string, string) {
   return b, a
}

// 判断x y的大小
func max(x, y int) int {
   if x > y {
      return x
   } else {
      return y
   }
}
```

## 数组

```go
package main

import "fmt"

func main() {
   //数组的声明
   var arr [5]int
   var arr2 [5]float32

   //初始化数组
   var arr3 = [5]int{11, 22, 33, 44, 55}
   arr4 := [5]int{11, 22, 33, 44, 55}
   arr5 := [...]string{"python", "java", "js"}

   //遍历arr5数组
   for i := 0; i < len(arr5); i++ {
      fmt.Printf("第%d个元素是：%s\n", i, arr5[i])
   }
    111
   //遍历arr5数组方法二
	for i, v := range arr5 {
		fmt.Printf("下标：%d 元素：%d\n", i, v)
	}
    
    

   println(arr[0])
   println(arr2[0])
   println(arr4[0])
   println(arr3[0])
}
```

## 结构体

```go
package main

import "fmt"

func main() {
   //定义结构体
   type Books struct {
      name   string
      money  float32
      author string
   }

   //创建实例
   book := Books{name: "《java从入门到入土》", money: 66, author: "Sky"}

   fmt.Printf("%s %f %s", book.name, book.money, book.author)

}
```

## 切片

```go
package main

import "fmt"

func main() {
   //声明一个切片
   var slice []string
   slice1 := make([]string, 10, 20)

   //初始化切片
   slice2 := []string{"sky", "666"}

   arr := [12]string{"I", "Love", "You", "And", "Thank", "You"}
   slice3 := arr[2:4]
   slice4 := arr[:4]
   slice5 := arr[3:]

   printSlice(slice)
   printSlice(slice1)
   printSlice(slice2)
   printSlice(slice3)
   printSlice(slice4)
   printSlice(slice5)
}

// 打印切片
func printSlice(x []string) {
   fmt.Printf("len=%d cap=%d Slic=%v \n", len(x), cap(x), x)
}
```

## 集合

```go
package main

import "fmt"

func main() {
   //声明集合
   m1 := make(map[string]string)
   m2 := map[string]string{"ct": "sky", "wb": "sb"}

   //空集合nil
   var m3 map[string]string
   var m4 = map[string]string{"ct": "sky", "wb": "sb"}

   //给集合赋值
   m1["Ct"] = "Sky"
   m1["Wb"] = "傻博"

   //打印
   printMap(m1)
   printMap(m2)
   printMap(m3)
   printMap(m4)
}

func printMap(m map[string]string) {
   //输出集合
   for name, nick := range m {
      fmt.Printf("name:%s nick:%s\n", name, nick)
   }
}
```

## 接口

```go
package main

type Phone interface {
   call()
}

type Iphone struct {
}

type xiaomi struct {
}

// 实现iphone的call方法
func (iphone14 Iphone) call() {
   println("我是iphone14")
}

// 实现小米 e的call方法
func (xiaomi xiaomi) call() {
   println("我是xiaomi")
}

func main() {
   //声明接口变量
   var phone Phone

   //创建一个iphone对象
   phone = new(Iphone)
   phone.call()

   //小米
   phone = new(xiaomi)
   phone.call()
}
```

## channel通道

### 管道基本使用

```go
package main

import "fmt"

func sum(s []int, c chan int) {
   sum := 0
   for _, v := range s {
      sum += v
   }
   c <- sum // 把 sum 发送到通道 c
}

func main() {
   s := []int{7, 2, 8, -9, 4, 0}

   c := make(chan int)
   go sum(s[:len(s)/2], c)
   go sum(s[len(s)/2:], c)
   x, y := <-c, <-c // 从通道 c 中接收

   fmt.Println(x, y, x+y)
}
```

### 通道关闭

```go
package main

func main() {
   c1 := make(chan int, 3)
   c1 <- 100
   c1 <- 111
   //关闭管道,无法写，只能读
   close(c1)
}
```

### 通道遍历

```go
package main

import "fmt"

func main() {
   intChan := make(chan int, 5)
   for i := 0; i < 5; i++ {
      intChan <- i * 2
   }

   //不关闭管道会死锁
   close(intChan)

   //遍历管道
   for v := range intChan {
      fmt.Printf("%v\n", v)
   }
}
```

### 只读只写管道

```go
package main

import "fmt"

func main() {
   //只读管道
   var redChan <-chan int
   redChan = make(chan int, 10)
   for v := range redChan {
      fmt.Printf("%v", v)
   }

   //只写管道
   var writeChan chan<- int
   writeChan = make(chan int, 10)
   writeChan <- 20
}
```

### select解决deadlock问题

### defer＋recover处理异常

```go
package main

func main() {
   var myMap map[string]string

   //异常处理
   defer func() {
      err := recover()
      if err != nil {
         println("出异常啦~")
      }
   }()

   //panic异常
   myMap["name"] = "sky"
}
```

## 并发编程

### 入门案例

```go
package main

import "time"

func Test() {
   for i := 0; i < 10; i++ {
      println("Test()", i)
      time.Sleep(time.Second)
   }
}

func main() {
   go Test()//开启协程

   for i := 0; i < 10; i++ {
      println("Main()", i)
      time.Sleep(time.Second)
   }
}
```

### runtime包

```go
package main

import "runtime"

func main() {
   //取cpu线程个数
   cpuN := runtime.NumCPU()
   //设置go最大线程个数
   runtime.GOMAXPROCS(cpuN - 2)
   println(cpuN)
}
```

### 资源竞争问题

> 全局变量加锁

### channel管道

```go
package main

import "fmt"

func main() {
	//创建int管道
	intChan := make(chan int, 3)
	fmt.Printf("管道内容：%v \n", intChan)
	//创建map管道
	mapChan := make(chan map[string]string, 3)
	fmt.Printf("管道内容：%v \n", mapChan)

	//管道添加数据
	intChan <- 10
	intChan <- 12
	fmt.Printf("原始长度：len:%d cap:%d \n", len(intChan), cap(intChan))

	mapChan <- map[string]string{"sky": "666", "sb": "333"}
	mapChan <- map[string]string{"sky": "666", "sb": "333"}
	mapChan <- map[string]string{"sky": "666", "sb": "333"}

	//读取管道数据
	num1 := <-intChan
	num2 := <-intChan
	fmt.Printf("数据：%d %d \n", num1, num2)
	fmt.Printf("取后长度：len:%d cap:%d \n", len(intChan), cap(intChan))

	map1 := <-mapChan
	map2 := <-mapChan
	fmt.Printf("数据：%v %v \n", map1, map2)
	fmt.Printf("取后长度：len:%d cap:%d \n", len(mapChan), cap(mapChan))
}

```

### 并行求素数【案例】

```go
package main

import (
   "fmt"
   "time"
)

func putNum(numChan chan int) {
   for i := 1; i < 500000; i++ {
      numChan <- i
   }
   close(numChan)
}

func getRes(numChan chan int, resChan chan int, exitChan chan bool) {
   var flag bool
   for {
      num, ok := <-numChan
      if !ok {
         break
      }
      flag = true
      for i := 2; i < num; i++ {
         if num%i == 0 {
            flag = false
            break
         }
      }

      if flag {
         resChan <- num
      }
   }

   exitChan <- true

}

func main() {
   //放置要求的数字
   numChan := make(chan int, 500000)
   //放置结果
   resChan := make(chan int, 500000)
   //放置各个协程结束情况
   exitChan := make(chan bool, 16)

   //初始化要求的数字
   go putNum(numChan)

   //开启16个协程
   for i := 0; i < 16; i++ {
      go getRes(numChan, resChan, exitChan)
   }

   start := time.Now().UnixMilli()
   //检测16个协程的运行状态，直到结束为止
   for i := 0; i < 16; i++ {
      <-exitChan
   }
   end := time.Now().UnixMilli()

   fmt.Printf("%v", end-start)

   //关闭
   close(resChan)

   //打印结果
   //for v := range resChan {
   // fmt.Printf("%v\n", v)
   //}
}
```

> 结果

```
十六个协程用时：
3073
3108
3111
八个协程用时：
3312
3370
3374
四个线程用时：
5795
5792
5844
一个协程用时：
22081
```

## 反射
