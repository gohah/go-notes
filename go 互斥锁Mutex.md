# go 互斥锁Mutex
go mutex是互斥锁，只有Lock和Unlock两个方法，在这两个方法之间的代码不能被多个goroutins同时调用到。
看代码：

```
package main

import (
	"fmt"
	"sync"
	"time"
)

var m *sync.Mutex

func main() {

	m = new(sync.Mutex)
	go lockPrint(1)
	lockPrint(2)
	time.Sleep(10*time.Second)
	fmt.Printf("%s\n", "exit!")

}

func lockPrint(i int) {

	println(i, "lock start")
	m.Lock()
	println(i, "in lock")
	time.Sleep(3 * time.Second)
	m.Unlock()
	println(i, "unlock")

}
```

解读：

> main函数里调用了两次lockPrint方法，这个方法中的println(i, "in lock")这句话，由于是在Mutex的Lock和Unlock之间，所以在第一次调用未被Unlock之前是不可能再被执行的。

结果：

 


```
2 lock start
2 in lock
1 lock start
2 unlock
1 in lock
1 unlock
exit!
```


从上面可以看到：第二行2 in lock打印以后，1 lock start已经进入调用了，但是直到2 unlock后 1才能in lock。

保证了Lock和Unlock之间的代码不能被同时调用。
