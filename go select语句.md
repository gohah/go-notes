# go select语句
select是Go中的一个控制结构，类似于用于通信的switch语句。每个case必须是一个通信操作，要么是发送要么是接收。
select随机执行一个可运行的case。如果没有case可运行，它将阻塞，直到有case可运行。一个默认的子句应该总是可运行的。

### 语法

Go 编程语言中 select 语句的语法如下：

```
select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s); 
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
```
以下描述了 select 语句的语法：

- 每个case都必须是一个通信
 
- 所有channel表达式都会被求值

- 所有被发送的表达式都会被求值

- 如果任意某个通信可以进行，它就执行；其他被忽略。
- 如果有多个case都可以运行，Select会随机公平地选出一个执行。其他不会执行。 

#### 否则：

- 如果有default子句，则执行该语句。
 
- 如果没有default字句，select将阻塞，直到某个通信可以运行；Go不会重新对channel或值进行求值。
 