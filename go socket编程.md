# go socket编程

- 服务端代码
```
package main

import(
	"fmt"
	"net"
)

func main() {
	fmt.Println("start server...")
	listen, err := net.Listen("tcp","0.0.0.0:50000")
	if err != nil {
		fmt.Println("listen failed,err:",err)
		return
	}
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn)
	}
}

func process(conn net.Conn) {
	defer conn.Close() 
	for {
		buf := make([]byte, 512) 
		_, err := conn.Read(buf) 
		if err != nil {
			fmt.Println("read err:", err)
			return 
		}
		fmt.Println("read: ", string(buf)) 
	}
}
```
- 客户端代码

```
package main
import ( 
	"bufio"
	"fmt" 
	"net" 
	"os" 
	"strings"
)
func main() {

	conn, err := net.Dial("tcp", "localhost:50000") 
	if err != nil {
		fmt.Println("Error dialing", err.Error())
		return 
	}
	defer conn.Close()
	inputReader := bufio.NewReader(os.Stdin) 
	for {
		input, _ := inputReader.ReadString('\n') 
		trimmedInput := strings.Trim(input, "\r\n") 
		if trimmedInput == "Q" {
			return 
		}
		_, err = conn.Write([]byte(trimmedInput)) 
		if err != nil {
			return 
		}
	} 
}
```

- http

```
package main

import (
	"fmt"
	"io"
	"net"
)

func main() {

	conn, err := net.Dial("tcp", "www.baidu.com:80")
	if err != nil {
		fmt.Println("Error dialing", err.Error())
		return
	}
	defer conn.Close()
	msg := "GET / HTTP/1.1\r\n"
	msg += "Host:www.baidu.com\r\n"
	msg += "Connection:keep-alive\r\n"
	//msg += "User-Agent:Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36\r\n"
	msg += "\r\n\r\n"

	//io.WriteString(os.Stdout, msg)
	n, err := io.WriteString(conn, msg)
	if err != nil {
		fmt.Println("write string failed, ", err)
		return
	}
	fmt.Println("send to baidu.com bytes:", n)
	buf := make([]byte, 4096)
	for {
		count, err := conn.Read(buf)
		fmt.Println("count:", count, "err:", err)
		if err != nil {
			break
		}
		fmt.Println(string(buf[0:count]))
	}
}
```


