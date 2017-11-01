# go sync.WaitGroup 
WaitGroup的用途：它能够一直等到所有的goroutine执行完成，并且阻塞主线程的执行，直到所有的goroutine执行完成。

WaitGroup总共有三个方法：Add(delta int),Done(),Wait()。简单的说一下这三个方法的作用。

Add:添加或者减少等待goroutine的数量

Done:相当于Add(-1)

Wait:执行阻塞，直到所有的WaitGroup数量变成0

例子：

```
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 5; i = i + 1 {
		wg.Add(1)
		go func(n int) {
			// defer wg.Done()
			defer wg.Add(-1)
			EchoNumber(n)
		}(i)
	}

	wg.Wait()
}

func EchoNumber(i int) {
	time.Sleep(3e9)
	fmt.Println(i)
}
```
输出结果：

```
0
1
2
3
4
```
程序很简单，只是将每次循环的数量过3秒钟输出。那么，这个程序如果不用WaitGroup，那么将看不见输出结果。因为goroutine还没执行完，主线程已经执行完毕。注释的defer wg.Done()和defer wg.Add(-1)作用一样。这个很好，原来执行脚本，都是使用time.Sleep，用一个估计的时间等到子线程执行完。WaitGroup很好。虽然chanel也能实现，但是觉得如果涉及不到子线程与主线程数据同步，这个感觉不错。
