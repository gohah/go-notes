> 冒泡排序


```
package main

import "fmt"
//冒泡排序
func bsort(a []int) {

       for i := 0; i < len(a); i++ {
              for j := 1; j < len(a)-i; j++ {
                     if a[j] < a[j-1] {
                            a[j], a[j-1] = a[j-1], a[j]
                     }
              }
       }
}

func main() {
       b := [...]int{8, 7, 5, 4, 3, 10, 15}
       bsort(b[:])
       fmt.Println(b)
}
```

> 选择排序


```
package main

import "fmt"
//选择排序
func ssort(a []int) {

       for i := 0; i < len(a); i++ {
              var min int = i
              for j := i + 1; j < len(a); j++ {
                     if a[min] > a[j] {
                            min = j
                     }
              }
              a[i], a[min] = a[min], a[i]
       }
}

func main() {
       b := [...]int{8, 7, 5, 4, 3, 10, 15}
       ssort(b[:])
       fmt.Println(b)
}
```

> 插入排序


```
package main

import "fmt"
//插入排序
func isort(a []int) {

       for i := 1; i < len(a); i++ {
              for j := i; j > 0; j-- {
                     if a[j] > a[j-1] {
                            break
                     }
                     a[j], a[j-1] = a[j-1], a[j]
              }
       }
}

func main() {
       b := [...]int{8, 7, 5, 4, 3, 10, 15}
       isort(b[:])
       fmt.Println(b)
}
```

> 快速排序


```
package main

import "fmt"
//快速排序
func qsort(a []int, left, right int) {
       if left >= right {
              return
       }

       val := a[left]
       k := left
       //确定val所在的位置
       for i := left + 1; i <= right; i++ {
              if a[i] < val {
                     a[k] = a[i]
                     a[i] = a[k+1]
                     k++
              }
       }

       a[k] = val
       qsort(a, left, k-1)
       qsort(a, k+1, right)
}

func main() {
       b := [...]int{8, 7, 5, 4, 3, 10, 15}
       qsort(b[:], 0, len(b)-1)
       fmt.Println(b)
}
```
