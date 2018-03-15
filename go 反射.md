# go 反射
```
package main

import (
       "fmt"
       "reflect"
)

type Student struct {
       Name  string
       Age   int
       Score float32
}

func test(b interface{}) {
       t := reflect.TypeOf(b)
       fmt.Println(t)

       v := reflect.ValueOf(b)
       k := v.Kind()
       fmt.Println(k)

       iv := v.Interface()
       stu, ok := iv.(Student)
       if ok {
              fmt.Printf("%v %T\n", stu, stu)
       }
}

func testInt(b interface{}) {
       val := reflect.ValueOf(b)
       //val.Elem()相当于 *val 
       val.Elem().SetInt(100)

       c := val.Elem().Int()
       fmt.Printf("get value  interface{} %d\n", c)
       fmt.Printf("string val:%d\n", val.Elem().Int())

}

func main() {
       var a Student = Student{
              Name:  "stu01",
              Age:   18,
              Score: 92,
       }
       test(a)

       var b int = 1
       b = 200
       //传递的是地址
       testInt(&b)
       fmt.Println(b)

}
```

```
main.Student
struct
{stu01 18 92} main.Student
get value  interface{} 100
string val:100
100
```


```
package main

import (
       "encoding/json"
       "fmt"
       "reflect"
)

type Student struct {
       Name  string `json:"student_name"`
       Age   int
       Score float32
       Sex   string
}

func (s Student) Print() {
       fmt.Println("---start----")
       fmt.Println(s)
       fmt.Println("---end----")
}

func (s Student) Set(name string, age int, score float32, sex string) {

       s.Name = name
       s.Age = age
       s.Score = score
       s.Sex = sex
}

func TestStruct(a interface{}) {
       tye := reflect.TypeOf(a)
       val := reflect.ValueOf(a)
       kd := val.Kind()
       if kd != reflect.Ptr && val.Elem().Kind() == reflect.Struct {
              fmt.Println("expect struct")
              return
       }

       num := val.Elem().NumField()
       val.Elem().Field(0).SetString("stu1000")
       for i := 0; i < num; i++ {
              fmt.Printf("%d %v\n", i, val.Elem().Field(i).Kind())
       }

       fmt.Printf("struct has %d fields\n", num)

       tag := tye.Elem().Field(0).Tag.Get("json")
       fmt.Printf("tag=%s\n", tag)

       numOfMethod := val.Elem().NumMethod()
       fmt.Printf("struct has %d methods\n", numOfMethod)
       var params []reflect.Value
       val.Elem().Method(0).Call(params)
}

func main() {
       var a Student = Student{
              Name:  "stu01",
              Age:   18,
              Score: 92.8,
       }

       result, _ := json.Marshal(a)
       fmt.Println("json result:", string(result))

       TestStruct(&a)
       fmt.Println(a)
}
```


```
json result: {"student_name":"stu01","Age":18,"Score":92.8,"Sex":""}
0 string
1 int
2 float32
3 string
struct has 4 fields
tag=student_name
struct has 2 methods
---start----
{stu1000 18 92.8 }
---end----
{stu1000 18 92.8 }
```

