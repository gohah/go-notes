# go 读写锁RWMutex
==读写锁==是针对于==读写操作==的==互斥锁==。

基本遵循两大原则：

> 1、可以随便读。多个goroutine同时读,但不能写。
> 
> 2、写的时候，啥都不能干。不能读，也不能写。
解释：

在32位的操作系统中，针对int64类型值的读操作和写操作不可能只由一个CPU指令完成。如果一个写的操作刚执行完了第一个指令，时间片换给另一个读的协程，这就会读到一个错误的数据。

 

RWMutex提供四个方法：

 

```
func (*RWMutex) Lock //写锁定

func (*RWMutex) Unlock //写解锁

func (*RWMutex) RLock //读锁定

func (*RWMutex) RUnlock //读解锁
```


 

代码实例：

1、可以随便读：

```
package main

import (
	"sync"
	"time"
)

var m sync.RWMutex

func main() {
	//可以多个同时读
	go read(1)
	go read(2)

	time.Sleep(2 * time.Second)
}

func read(i int) {

	println(i, "read start")
	m.RLock()
	println(i, "reading")
	time.Sleep(1 * time.Second)
	m.RUnlock()
	println(i, "read end")

}
```
运行结果：


```
1 read start

1 reading

2 read start

2 reading

1 read end

2 read end
```


> 可以看到1读还没结束（倒数第二行）的时候，2已经在读（倒数第三行）了。

2、写的时候啥也不能干：

```
package main

import (
	"sync"
	"time"
)

var m sync.RWMutex

func main() {

	//写的时候啥都不能干
	go write(1)
	go read(2)
	go write(3)

	time.Sleep(4 * time.Second)

}

func read(i int) {

	println(i, "read start")
	m.RLock()
	println(i, "reading")
	time.Sleep(1 * time.Second)
	m.RUnlock()
	println(i, "read end")

}

func write(i int) {

	println(i, "write start")
	m.Lock()
	println(i, "writing")
	time.Sleep(1 * time.Second)
	m.Unlock()
	println(i, "write end")

}

```

输出：

 


```
1 write start

1 writing

2 read start

3 write start

1 write end

2 reading

2 read end

3 writing

3 write end
```


可以看到：

> 1、1 write end结束之后，2才能reading
> 
> 2、2 read end结束之后，3 才能writing