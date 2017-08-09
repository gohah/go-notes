Go语言通过使用标准库里的flag包来处理命令行参数。

Package flag implements command-line flag parsing.

http://golang.org/pkg/flag/

http://golang.org/pkg/

几点注意事项：

> 1，通过flag.String(), Bool(), Int()等方式来定义命令行中需要使用的flag。

> 2，在定义完flag后，通过调用flag.Parse()来进行对命令行参数的解析。

> 3，命令行参数的格式可以是：
> 
> -flag xxx （使用空格，一个 - 符号）
> 
> --flag xxx （使用空格，两个 - 符号）
> 
> -flag=xxx （使用等号，一个 - 符号）
> 
> --flag=xxx （使用等号，两个 - 符号）
> 
> 其中，布尔类型的参数防止解析时的二义性，应该使用等号的方式指定。


```
package main

import (
       "flag" // command line option parser
       "fmt"
)

func main() {

       var test bool
       var str string
       var count int
       flag.BoolVar(&test, "b", false, "print on newline")
       flag.StringVar(&str, "s", "", "print on newline")
       flag.IntVar(&count, "c", 1001, "print on newline")
       flag.Parse()

       fmt.Println(test)
       fmt.Println(str)
       fmt.Println(count)
}
```
