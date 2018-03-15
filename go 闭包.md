# go 闭包

```
package main

import(
	"fmt"
)

func Adder() func(int)int {
	var x int
	f := func(i int)int {
		x = x + i
		return x
	}
	return f
}

func TestClosure() {
	f := Adder()
	fmt.Println(f(10))
	fmt.Println(f(20))
	fmt.Println(f(30))

	f1 := Adder()
	fmt.Println(f1(10))
	fmt.Println(f1(20))
	fmt.Println(f1(30))
}

func main() {
	TestClosure()
}
```


```
10
30
60
10
30
60
```

