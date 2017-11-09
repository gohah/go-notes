
```
package main

import (
       "fmt"
)

func calc(taskChan chan int, resChan chan int,
       exitChan chan bool) {
       for v := range taskChan {
              flag := true
              for i := 2; i < v; i++ {
                     if v%i == 0 {
                            flag = false
                            break
                     }
              }

              if flag {
                     resChan <- v
              }
       }

       fmt.Println("exit")
       exitChan <- true
}

func main() {
       intChan := make(chan int, 1000)
       resultChan := make(chan int, 1000)
       exitChan := make(chan bool, 8)

       go func() {
              for i := 0; i < 10000; i++ {
                     intChan <- i
              }

              close(intChan)
       }()

       for i := 0; i < 8; i++ {
              go calc(intChan, resultChan, exitChan)
       }

       //等待所有计算的goroutine全部退出
       go func() {
              for i := 0; i < 8; i++ {
                     <-exitChan
                     fmt.Println("wait goroute ", i, " exited")
              }
              close(resultChan)
       }()

       for v := range resultChan {
              fmt.Println(v)
       }
}
```


```
package main

import "fmt"

func send(ch chan int, exitChan chan struct{}) {

       for i := 0; i < 10; i++ {
              ch <- i
       }

       close(ch)
       var a struct{}
       exitChan <- a
}

func recv(ch chan int, exitChan chan struct{}) {
       for {
              v, ok := <-ch
              if !ok {
                     break
              }
              fmt.Println(v)
       }

       var a struct{}
       exitChan <- a
}

func main() {
       var ch chan int
       ch = make(chan int, 10)
       exitChan := make(chan struct{}, 2)

       go send(ch, exitChan)
       go recv(ch, exitChan)

       var total = 0
       for _ = range exitChan {
              total++
              if total == 2 {
                     break
              }
       }
}

```

```
package main

import (
	"fmt"
	"time"
)

func main() {
	a := make(chan int, 5)

	go func(){
		for _,i := range []int{1,2,3,4,5} {
			a <- i
		}
	}()

	//go func(){
	//	for{
	//		i,ok := <-a
	//		fmt.Println(i,ok)
	//
	//		if !ok {
	//			break
	//		}
	//	}
	//}()

	go func(){
		for v := range a{

			fmt.Println(v)
		}
	}()

	time.Sleep(5*time.Second)
	close(a)
	time.Sleep(5*time.Second)

}

```

> channel的关闭
使用内置函数close进行关闭，chan关闭之后，for range遍历chan中
已经存在的元素后结束

> 使用内置函数close进行关闭，chan关闭之后，没有使用for range的写法
> 需要使用，v, ok := <- ch进行判断chan是否关闭



