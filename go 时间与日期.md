# go 时间与日期

```
package main

import(
	"fmt"
	"time"
)
func TestTime() {
	now := time.Now()
	year := now.Year()
	month := now.Month()
	day := now.Day()
	hour := now.Hour()
	minute := now.Minute()
	second := now.Second()

	fmt.Printf("%04d-%02d-%02d %02d:%02d:%02d\n",year, month,day, hour,minute,second)
	fmt.Println(now.Unix())
}
func TestTimeConst() {
	fmt.Printf("Nanosecond: %d\n",time.Nanosecond)
	fmt.Printf("Microsecond:%d\n",time.Microsecond)
	fmt.Printf("Millisecond:%d\n",time.Millisecond)
	fmt.Printf("Second:     %d\n",time.Second)
	fmt.Printf("Minute:     %d\n",time.Minute)
	fmt.Printf("Hour:       %d\n",time.Hour)
}
func TestTimeFormat() {
	now := time.Now()
	nowStr := now.Format("2006-01-02 15:04:05")
	fmt.Println(nowStr)
}
func main() {
	TestTime()
	TestTimeConst()
	TestTimeFormat()
}
```

```
2018-02-27 20:33:04
1519734784
Nanosecond: 1
Microsecond:1000
Millisecond:1000000
Second:     1000000000
Minute:     60000000000
Hour:       3600000000000
2018-02-27 20:33:04
```
