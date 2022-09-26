# Go语言基础

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

## 通道

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