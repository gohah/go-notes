# go defer

```
package main

import(
	"fmt"
)

func TestDefer() {
	var a int = 100
	fmt.Printf("befor defer: a=%d\n",a)
	defer fmt.Printf("defering 1: a=%d\n",a)
	if a > 100 {
		return
	}
	a = 200
	defer fmt.Printf("defering 2: a=%d\n",a)
	fmt.Printf("after defer: a=%d\n",a)
}

func main() {
	TestDefer()
}
```


```
befor defer: a=100
after defer: a=200
defering 2: a=200
defering 1: a=100
```

