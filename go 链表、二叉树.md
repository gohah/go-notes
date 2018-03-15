# go 链表、二叉树
> 链表


```
//例子1
package main

import (
       "fmt"
       "math/rand"
)

type Student struct {
       Name  string
       Age   int
       Score float32
       next  *Student
}
//遍历节点
func trans(p *Student) {
       for p != nil {
              fmt.Println(*p)
              p = p.next
       }

       fmt.Println()
}
//从尾部插入
func insertTail(p *Student) {
       var tail = p
       for i := 0; i < 10; i++ {
              stu := Student{
                     Name:  fmt.Sprintf("stu%d", i),
                     Age:   rand.Intn(100),
                     Score: rand.Float32() * 100,
              }

              tail.next = &stu
              tail = &stu
       }
}
//从头部插入
func insertHead(p **Student) {
       //var tail = p
       for i := 0; i < 10; i++ {
              stu := Student{
                     Name:  fmt.Sprintf("stu%d", i),
                     Age:   rand.Intn(100),
                     Score: rand.Float32() * 100,
              }

              stu.next = *p
              *p = &stu
       }
}
//删除节点
func delNode(p *Student) {

       var prev *Student = p
       for p != nil {
              if p.Name == "stu6" {
                     prev.next = p.next
                     break
              }
              prev = p
              p = p.next
       }
}
//添加节点
func addNode(p *Student, newNode *Student) {

       for p != nil {
              if p.Name == "stu9" {
                     newNode.next = p.next
                     p.next = newNode
                     break
              }

              p = p.next
       }
}

func main() {
       var head *Student = new(Student)

       head.Name = "hua"
       head.Age = 18
       head.Score = 100

       //insertTail(head)
       //trans(head)
       insertHead(&head)
       trans(head)

       delNode(head)
       trans(head)

       var newNode *Student = new(Student)

       newNode.Name = "stu1000"
       newNode.Age = 18
       newNode.Score = 100
       addNode(head, newNode)
       trans(head)
}
```


```
//例子2

package main

import "fmt"

//节点
type LinkNode struct {
       data interface{}
       next *LinkNode
}
//链表
type Link struct {
       head *LinkNode
       tail *LinkNode
}

//从头部插入
func(p *Link)InsertHead(data interface{}) {
       linkNode := &LinkNode{
              data:data,
              next:nil,
       }

       if p.tail == nil && p.head == nil {
              p.tail = linkNode
              p.head = linkNode
              return
       }

       linkNode.next = p.head
       p.head = linkNode
}

//从尾部插入
func(p *Link)InsertTail(data interface{}) {
       linkNode := &LinkNode{
              data: data,
              next: nil,
       }

       if p.tail == nil && p.head == nil {
              p.tail = linkNode
              p.head = linkNode
              return
       }

       p.tail.next = linkNode
       p.tail = linkNode
}
//遍历节点
func (p *Link) Trans() {
       q := p.head
       for q != nil {
              fmt.Println(q.data)
              q = q.next
       }
}


package main

import "fmt"

func main() {

       var link Link
       for i := 0; i < 10; i++ {
              link.InsertTail(fmt.Sprintf("str %d", i))
       }

       link.Trans()
}
```

```
//二叉树

package main

import "fmt"

type Student struct {
       Name  string
       Age   int
       Score float32
       left  *Student
       right *Student
}
//遍历二叉树
func trans(root *Student) {
       if root == nil {
              return
       }
       fmt.Println(root)

       trans(root.left)
       trans(root.right)

}

func main() {
       var root *Student = new(Student)

       root.Name = "stu01"
       root.Age = 18
       root.Score = 100

       var left1 *Student = new(Student)
       left1.Name = "stu02"
       left1.Age = 18
       left1.Score = 100

       root.left = left1

       var right1 *Student = new(Student)
       right1.Name = "stu04"
       right1.Age = 18
       right1.Score = 100

       root.right = right1

       var left2 *Student = new(Student)
       left2.Name = "stu03"
       left2.Age = 18
       left2.Score = 100

       left1.left = left2

       trans(root)
}
```

