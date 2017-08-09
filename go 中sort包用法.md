golang中也实现了排序算法的包sort包．

sort包中实现了３种基本的排序算法：插入排序．快排和堆排序．和其他语言中一样，这三种方式都是不公开的，他们只在sort包内部使用．所以用户在使用sort包进行排序时无需考虑使用那种排序方式，sort.Interface定义的三个方法：获取数据集合长度的Len()方法、比较两个元素大小的Less()方法和交换两个元素位置的Swap()方法，就可以顺利对数据集合进行排序。sort包会根据实际数据自动选择高效的排序算法。

# type Interface


```
type Interface interface {

    Len() int    // Len 为集合内元素的总数
  
    Less(i, j int) bool　//如果index为i的元素小于index为j的元素，则返回true，否则返回false

    Swap(i, j int)  // Swap 交换索引为 i 和 j 的元素
}
```

任何实现了 sort.Interface 的类型（一般为集合），均可使用该包中的方法进行排序。这些方法要求集合内列出元素的索引为整数。


```
package main

import (
       "fmt"
       "math/rand"
       "sort"
)

type Person struct {
       Name string
       Age int
       Score float32
}

type PersonArr []Person

func(p PersonArr)Len() int {
       return len(p)
}

func(p PersonArr)Less(i,j int)bool {
       return p[i].Name < p[j].Name
}

func(p PersonArr)Swap(i,j int) {
       p[i],p[j] = p[j],p[i]
}

func main() {
       var pArr PersonArr

       for i:=0; i<10; i++ {
              p:= Person{
                     Name:fmt.Sprintf("stu%d",rand.Intn(100)),
                     Age:rand.Int(),
                     Score:rand.Float32(),
              }
              pArr = append(pArr, p)
       }

       for _,v:= range(pArr) {
              fmt.Println(v)
       }

       sort.Sort(pArr)

       fmt.Println()

       for _,v:= range(pArr) {
              fmt.Println(v)
       }
}
```
