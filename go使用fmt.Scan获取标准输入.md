Go fmt包下有三个函数，可以在程序运行过程中获取用户输入。
> fmt.Scan：获取输入
> fmt.Scanf：获取输入，但是可以指定格式，go会根据格式解析参数
> fmt.Scanln：获取一行的输入，只会获取到一行。

> 示例，我们需要和gates和jobs问个好，代码：


```
fmt.Println("Please enter your firstname and lastname ")
var a1, a2 string
fmt.Scan(&a1, &a2)
fmt.Println("hello,", a1, "and", a2)
```

运行后，在窗口中输入：

gates jobs
输出：
hello, gates and jobs
可以看出，go把输入的参数按空格分开后，分别赋值给了a1和a2。
整体运行结果（第二行是运行时用户输入的）：
Please enter your names
gates jobs
hello, gates and jobs

如果我们输入时换行输入：
> Please enter your names
> gates
> jobs
> hello, gates and jobs
可以看出gates和jobs中间是敲了回车的，这是Scan和Scanln的区别。Scanln只要收到回车就不会继续接收输入了。

> 示例2：


```
fmt.Println("Please enter your names")
var b1, b2 string
fmt.Scanf("%s , %s", &b1, &b2)
fmt.Println("hello,", b1, "and", b2)
```

运行结果：

> Please enter your names
> gates , jobs
> hello, gates and jobs

上面的示例，需要注意两点：
1、Scanf中间有一个逗号，但逗号和%s间有空格，因为Scanf是用空格来区分不同的参数的。
2、输入的参数gates , jobs格式与Scanf中指定的fmt要一致。
3、中间的逗号，Scanf会自动格式匹配不会添加到变量中

示例3：
Scanln和Scan非常类似，只是Scanln只会接受一个回车，收到回车就扫描结束了。


```
var c1, c2 string
fmt.Scanln(&c1, &c2)
fmt.Println("hello,", c1, "and", c2)
```

运行结果：

> Please enter your names
> gates jobs
> hello, gates and jobs

如果换行输入，结果是：
> Please enter your names
> gates
> hello, gates and
因为输入了gates后，回车，结果就打印出来了，没机会再输入jobs了，和Scan函数不一样。