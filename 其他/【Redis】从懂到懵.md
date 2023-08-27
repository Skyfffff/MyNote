# RedisGo

## 操作string

```go
package main

import (
   "fmt"
   "redigo-master/redis"
)

func main() {
   //连接redis数据库
   conn, err := redis.Dial("tcp", "localhost:6379")
   if err != nil {
      fmt.Printf("%v", err)
   }
   //关闭连接
   defer func(conn redis.Conn) {
      err := conn.Close()
      if err != nil {
         fmt.Printf("%v", err)
      }
   }(conn)

   //添加string数据
   _, err = conn.Do("set", "name", "sky")
   if err != nil {
      fmt.Printf("%v", err)
   }

   //读取数据
   r, err := redis.String(conn.Do("get", "name"))
   if err != nil {
      fmt.Printf("%v", err)
   }
   fmt.Printf("%v", r)
}
```

## 操作Hash

```go
package main

import (
   "fmt"
   "redigo-master/redis"
)

func main() {
   //连接redis数据库
   conn, err := redis.Dial("tcp", "localhost:6379")
   if err != nil {
      fmt.Printf("%v", err)
   }
   //关闭连接
   defer func(conn redis.Conn) {
      err := conn.Close()
      if err != nil {
         fmt.Printf("%v", err)
      }
   }(conn)

   //添加map数据
   _, err = conn.Do("hset", "user", "name", "sky")
   if err != nil {
      fmt.Printf("%v", err)
   }

   //读取数据
   r, err := redis.String(conn.Do("hget", "user", "name"))
   if err != nil {
      fmt.Printf("%v", err)
   }
   fmt.Printf("%v", r)
}
```

## redis连接池

```go
package main

import (
   "fmt"
   "redigo-master/redis"
)

var pool *redis.Pool

func init() {
   pool = &redis.Pool{
      Dial: func() (redis.Conn, error) {
         return redis.Dial("tcp", "localhost:6379")
      }, //连接
      DialContext:     nil,
      TestOnBorrow:    nil,
      MaxIdle:         8,   //最大空闲连接数
      MaxActive:       0,   //最大数据库连接数，0表示没有限制
      IdleTimeout:     100, //最大超时时间
      Wait:            false,
      MaxConnLifetime: 0,
   }
}

func main() {
   //获得一个连接
   conn := pool.Get()
   //延迟关闭连接
   defer func(conn redis.Conn) {
      err := conn.Close()
      if err != nil {
         fmt.Printf("%v", err)
      }
   }(conn)

   //操作redis
   _, err := conn.Do("set", "name", "Sky")
   if err != nil {
      fmt.Printf("%v", err)
   }
   //获取数据
   r, err := redis.String(conn.Do("get", "name"))
   if err != nil {
      fmt.Printf("%v", err)
   }
   fmt.Printf("%s", r)
}
```
