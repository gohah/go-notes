# go 常量使用 iota
iota是golang语言的常量计数器,只能在常量的表达式中使用。

iota在const关键字出现时将被重置为0(const内部的第一行之前)，const中每新增一行常量声明将使iota计数一次(iota可理解为const语句块中的行索引)。

使用iota能简化定义，在定义枚举时很有用。

举例如下：

1、iota只能在常量的表达式中使用。


```
fmt.Println(iota)  
编译错误： undefined: iota
```


 

2、每次 const 出现时，都会让 iota 初始化为0.


```
const a = iota // a=0 
const ( 
  b = iota     //b=0 
  c            //c=1 
)
```

 

3、自定义类型

自增长常量经常包含一个自定义枚举类型，允许你依靠编译器完成自增设置。


```
type Stereotype int

const ( 
    TypicalNoob Stereotype = iota // 0 
    TypicalHipster                // 1 
    TypicalUnixWizard             // 2 
    TypicalStartupFounder         // 3 
)
```

 

4、可跳过的值

设想你在处理消费者的音频输出。音频可能无论什么都没有任何输出，或者它可能是单声道，立体声，或是环绕立体声的。 
这可能有些潜在的逻辑定义没有任何输出为 0，单声道为 1，立体声为 2，值是由通道的数量提供。 
所以你给 Dolby 5.1 环绕立体声什么值。 
一方面，它有6个通道输出，但是另一方面，仅仅 5 个通道是全带宽通道（因此 5.1 称号 - 其中 .1 表示的是低频效果通道）。 
不管怎样，我们不想简单的增加到 3。 
我们可以使用下划线跳过不想要的值。


```
type AudioOutput int

const ( 
    OutMute AudioOutput = iota // 0 
    OutMono                    // 1 
    OutStereo                  // 2 
    _ 
    _ 
    OutSurround                // 5 
)
```

 

5、位掩码表达式


```
type Allergen int

const ( 
    IgEggs Allergen = 1 << iota // 1 << 0 which is 00000001 
    IgChocolate                         // 1 << 1 which is 00000010 
    IgNuts                              // 1 << 2 which is 00000100 
    IgStrawberries                      // 1 << 3 which is 00001000 
    IgShellfish                         // 1 << 4 which is 00010000 
)
```


这个工作是因为当你在一个 const 组中仅仅有一个标示符在一行的时候，它将使用增长的 iota 取得前面的表达式并且再运用它，。在 Go 语言的 spec 中， 这就是所谓的隐性重复最后一个非空的表达式列表。

如果你对鸡蛋，巧克力和海鲜过敏，把这些 bits 翻转到 “on” 的位置（从左到右映射 bits）。然后你将得到一个 bit 值 00010011，它对应十进制的 19。

fmt.Println(IgEggs | IgChocolate | IgShellfish)

// output: 
// 19

 

6、定义数量级


```
type ByteSize float64

const (
    _           = iota                   // ignore first value by assigning to blank identifier
    KB ByteSize = 1 << (10 * iota) // 1 << (10*1)
    MB                                   // 1 << (10*2)
    GB                                   // 1 << (10*3)
    TB                                   // 1 << (10*4)
    PB                                   // 1 << (10*5)
    EB                                   // 1 << (10*6)
    ZB                                   // 1 << (10*7)
    YB                                   // 1 << (10*8)
)
```

 

7、定义在一行的情况


```
const (
    Apple, Banana = iota + 1, iota + 2
    Cherimoya, Durian
    Elderberry, Fig
)
iota 在下一行增长，而不是立即取得它的引用。

// Apple: 1 
// Banana: 2 
// Cherimoya: 2 
// Durian: 3 
// Elderberry: 3 
// Fig: 4
```


 

8、中间插队


```
const ( 
    i = iota 
    j = 3.14 
    k = iota 
    l 
)

那么打印出来的结果是 i=0,j=3.14,k=2,l=3
```
