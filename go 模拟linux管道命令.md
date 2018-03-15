# go模拟linux管道命令

- 示例1
```
//匿名管道
package main

import (
	"os/exec"
	"fmt"
	"io"
)

func main() {

	cmd := exec.Command("echo","-n","hello,world")

	stdout,err := cmd.StdoutPipe()
	if err != nil {
		fmt.Printf("Error: couldn't obtain the stdout pipe for command No.0: %s",err)
		return
	}

	if err:= cmd.Start();err != nil {
		fmt.Printf("Error: the command No.0 can not be startup: %s",err)
		return
	}

	for{
		tmp := make([]byte,5)

		n,err := stdout.Read(tmp)

		if err != nil {
			if err == io.EOF {
				break
			} else {
				fmt.Printf("Error: Couldn't read data from the pipe: %s\n",err)
				return
			}

		}

		fmt.Printf(string(tmp[:n]))
	}


}

```
- 示例2

```
//匿名管道
package main

import (
	"os/exec"
	"bytes"
	"fmt"
)
// ps aux | grep 80
func main() {
	cmd1 := exec.Command("ps","aux")
	cmd2 := exec.Command("grep","80")

	var stdout1 bytes.Buffer
	cmd1.Stdout = &stdout1

	if err := cmd1.Start(); err != nil {
		return
	}

	if err := cmd1.Wait(); err != nil {
		return
	}

	var stdout2 bytes.Buffer
	cmd2.Stdin = &stdout1
	cmd2.Stdout = &stdout2

	if err := cmd2.Start(); err != nil {
		return
	}

	if err := cmd2.Wait(); err != nil {
		return
	}

	fmt.Println(string(stdout2.Bytes()))
}

```

- 示例3

```

```


