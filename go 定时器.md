# go 定时器
```
package main

import (
       "time"
       "fmt"
)
//每隔一秒执行一次
func main() {
       t := time.NewTicker(time.Second)

       for v := range t.C {
              fmt.Println(v)
       }
}
```


```
package main

import (
       "time"
       "fmt"
)
//一次定时
func main() {
       select {
       case <- time.After(time.Second):
              fmt.Println("after one second")
       }
}
```


```
package main

import (
       "time"
       "fmt"
)
//超时控制
func queryDb(ch chan int) {
       time.Sleep(time.Second)
       ch <- 100
}

func main() {
       ch := make(chan int)

       go queryDb(ch)

       t := time.NewTicker(time.Second)

       select {
       case v := <-ch:
              fmt.Println(v)
       case <- t.C:
              fmt.Println("timeout")
       }
}
```











