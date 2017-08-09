
```
//student.go
package main

import (
       "encoding/json"
       "io/ioutil"
)

type student struct {
       Name string
       Sex  string
       Age  int
}

func (p *student) Save() (err error) {
       data, err := json.Marshal(p)
       if err != nil {
              return
       }

       err = ioutil.WriteFile("C:/stu.dat", data, 0755)
       return
}

func (p *student) Load() (err error) {

       data, err := ioutil.ReadFile("C:/stu.dat")
       if err != nil {
              return
       }

       err = json.Unmarshal(data, p)
       return
}

```

```
//student_test.go
package main

import "testing"
import "time"

func TestSave(t *testing.T) {
       stu := &student{
              Name: "stu01",
              Sex:  "man",
              Age:  10,
       }

       err := stu.Save()
       if err != nil {
              t.Fatalf("save student failed, err:%v", err)
       }

}

func TestLoad(t *testing.T) {

       stu := &student{
              Name: "stu01",
              Sex:  "man",
              Age:  10,
       }
       err := stu.Save()
       if err != nil {
              t.Fatalf("save student failed, err:%v", err)
       }
       stu2 := &student{}
       time.Sleep(10 * time.Second)
       err = stu2.Load()
       if err != nil {
              t.Fatalf("load student failed, err:%v", err)
       }
       if stu.Name != stu2.Name {
              t.Fatalf("load student failed, name not equal")
       }
       if stu.Sex != stu2.Sex {
              t.Fatalf("load student failed, Sex not equal")
       }
       if stu.Age != stu2.Age {
              t.Fatalf("load student failed, Age not equal")
       }
}
```





