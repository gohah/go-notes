# go redis
redis是个开源的⾼性能的key-value的内存数据库，可以把它当成远程
的数据结构。

⽀持的value类型⾮常多，⽐如string、list（链表）、set（集合）、
hash表等等

redis性能⾮常⾼，单机能够达到15w qps，通常适合做缓存。

- 使⽤第三⽅开源的redis库:github.com/garyburd/redigo/redis


```
import(
 "github.com/garyburd/redigo/redis"
)
```
- 链接redis
```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
}
```

- Set 接⼝
```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
     _, err = c.Do("Set", "abc", 100)
     if err != nil {
         fmt.Println(err)
         return
     }
     r, err := redis.Int(c.Do("Get", "abc"))
     if err != nil {
         fmt.Println("get abc failed,", err)
         return
     }
     fmt.Println(r)
}
```

- Hash表
```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
     _, err = c.Do("HSet", "books", "abc", 100)
     if err != nil {
         fmt.Println(err)
         return
     }
     r, err := redis.Int(c.Do("HGet", "books", "abc"))
     if err != nil {
         fmt.Println("get abc failed,", err)
         return
     }
     fmt.Println(r)
}
```
- 批量Set

```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
     _, err = c.Do("MSet", "abc", 100, "efg", 300)
     if err != nil {
         fmt.Println(err)
         return
     }
     r, err := redis.Ints(c.Do("MGet", "abc", "efg"))
     if err != nil {
         fmt.Println("get abc failed,", err)
         return
     }
     for _, v := range r {
        fmt.Println(v)
     }
}
```
- 过期时间

```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
     _, err = c.Do("expire", "abc", 10)
     if err != nil {
         fmt.Println(err)
         return
     }
}
```

- 队列操作
```
package main
import (
     "fmt"
     "github.com/garyburd/redigo/redis"
)
func main() {
     c, err := redis.Dial("tcp", "localhost:6379")
     if err != nil {
         fmt.Println("conn redis failed,", err)
         return
     }
     defer c.Close()
     _, err = c.Do("lpush", "book_list", "abc", "ceg", 300)
     if err != nil {
         fmt.Println(err)
         return
     }
     r, err := redis.String(c.Do("lpop", "book_list"))
     if err != nil {
         fmt.Println("get abc failed,", err)
         return
     }
     fmt.Println(r)
}
```






