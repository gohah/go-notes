

Go语言追求简洁优雅，所以，go语言不支持传统的 try…catch…finally 这种异常，因为Go语言的设计者们认为，将异常与控制结构混在一起会很容易使得代码变得混乱。因为开发者很容易滥用异常，甚至一个小小的错误都抛出一个异常。在Go语言中，使用多值返回来返回错误。不要用异常代替错误，更不要用来控制流程。在极个别的情况下，也就是说，遇到真正的异常的情况下（比如除数为0了）。才使用Go中引入的Exception处理：defer, panic, recover。

这几个异常的使用场景可以这么简单描述：Go中可以抛出一个panic的异常，然后在defer中通过recover捕获这个异常，然后正常处理。




```
package main

import "fmt"

func main(){
       defer func(){ // 必须要先声明defer，否则不能捕获到panic异常
              fmt.Println("c")
              if err:=recover();err!=nil{
                     fmt.Println(err) // 这里的err其实就是panic传入的内容，55
              }
              fmt.Println("d")
       }()
       f()
}

func f(){
       fmt.Println("a")
       panic(55)
       fmt.Println("b")
       fmt.Println("f")
}
```

运行结果：

> a
> c
> 55
> d
> exit code 0, process exited normally.

参考： http://blog.csdn.net/ghost911_slb/article/details/7831574

