# go 结构体tag
```
package main

import (
       "encoding/json"
       "fmt"
)
//结构体TAG
type student struct {
       Name string `json:"name"`

       Age int `json:"age"`
}


func main() {
       var stu student = student{
              Name:"huangwei",
              Age:30,
       }

       data, err := json.Marshal(stu)

       fmt.Println(string(data),err)
}
```

运行结果：

> {"name":"huangwei","age":30} <nil>