# go 随机数


```
package main

import(
	"fmt"
	"math/rand"
	"time"
)

func main() {
    //种子，没有种子的话随机数是相同的
	r := rand.New(rand.NewSource(time.Now().Unix())) 
	
	fmt.Println(r.Int())
	fmt.Println(r.Intn(100))
	fmt.Println(r.Float32())
}
```
