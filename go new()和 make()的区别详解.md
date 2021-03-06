# go new()和 make()的区别详解
### 概述
Go 语言中的 new 和 make 一直是新手比较容易混淆的东西，咋一看很相似。不过解释两者之间的不同也非常容易。
### new 的主要特性
首先 new 是内建函数，你可以从 http://golang.org/pkg/builtin/#new 这儿看到它，它的定义也很简单：

```
func new(Type) *Type
```
官方文档对于它的描述是：

```
内建函数 new 用来分配内存，它的第一个参数是一个类型，不是一个值，它的返回值是一个指向新分配类型零值的指针
```
根据这段描述，我们可以自己实现一个类似 new 的功能：

```
func newInt() *int {
  var i int
  return &i
}
someInt := newInt()
```

我们这个函数的功能跟 someInt := new(int) 一模一样。所以在我们自己定义 new 开头的函数时，出于约定也应该返回类型的指针。

### make 的主要特性

make 也是内建函数，你可以从 http://golang.org/pkg/builtin/#make 这儿看到它，它的定义比 new 多了一个参数，返回值也不同：

```
func make(Type, size IntegerType) Type
```

官方文档对于它的描述是：
内建函数 make 用来为 slice，map 或 chan 类型分配内存和初始化一个对象(注意：只能用在这三种类型上)，跟 new 类似，第一个参数也是一个类型而不是一个值，跟 new 不同的是，make 返回类型的引用而不是指针，而返回值也依赖于具体传入的类型，具体说明如下：

```
Slice: 第二个参数 size 指定了它的长度，它的容量和长度相同。
你可以传入第三个参数来指定不同的容量值，但必须不能比长度值小。
比如 make([]int, 0, 10)

Map: 根据 size 大小来初始化分配内存，不过分配后的 map 长度为 0，如果 size 被忽略了，那么会在初始化分配内存时分配一个小尺寸的内存

Channel: 管道缓冲区依据缓冲区容量被初始化。如果容量为 0 或者忽略容量，管道是没有缓冲区的
```
### 总结
new 的作用是初始化一个指向类型的指针(*T)

make 的作用是为 slice，map 或 chan 初始化并返回引用(T)。


```
package main


import(
	"fmt"
)

func main() {
	var a []int
	a = make([]int,10)
	a[0] = 100
	fmt.Println(a)

	var p *[]int
	p = new([]int) //new 为p分配内存(初始化)
	*p = make([]int,10) //make 为切片分配内存(初始化)
	(*p)[0] = 100
	fmt.Println(p)

	p = &a
	(*p)[0] = 1000
	fmt.Println(a)
}
```

```
[100 0 0 0 0 0 0 0 0 0]
&[100 0 0 0 0 0 0 0 0 0]
[1000 0 0 0 0 0 0 0 0 0]
```





