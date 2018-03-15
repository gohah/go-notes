# go goroutine中使用recover
> 如果某个goroutine panic了，而且这个goroutine里面没有捕获(recover)，那么整个进程就会挂掉。所以，好的习惯是每当go产生一个goroutine，就需要写下recover


```
package main

import (
       "fmt"
       "runtime"
       "time"
)

func test() {

       defer func() {
              if err := recover(); err != nil {
                     fmt.Println("panic:", err)
              }
       }()

       var m map[string]int
       m["stu"] = 100
}

func calc() {
       for {
              fmt.Println("i'm calc")
              time.Sleep(time.Second)
       }
}

func main() {
       num := runtime.NumCPU()
       runtime.GOMAXPROCS(num - 1)
       go test()
       for i := 0; i < 2; i++ {
              go calc()
       }

       time.Sleep(time.Second * 10000)
}
```
