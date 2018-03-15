# go 文件读写
```
//方式一（一行一行读取）
package main

import (
       "bufio"
       "fmt"
       "io"
       "os"
)

func main() {

       inputFile, err := os.Open("input.dat")
       if err != nil {
              fmt.Printf("open file err:%v\n", err)
              return
       }

       defer inputFile.Close()
       
       inputReader := bufio.NewReader(inputFile)
       
       for {
              inputString, readerError := inputReader.ReadString('\n')
              if readerError == io.EOF {
                     return
              }
              fmt.Printf("The input was: %s", inputString)
       }
}
```


```
//方式二（读取整个文件）
package main

import (
       "fmt"
       "io/ioutil"
       "os"
)

func main() {

       inputFile := "products.txt"
       outputFile := "products_copy.txt"
       buf, err := ioutil.ReadFile(inputFile)
       if err != nil {
              fmt.Fprintf(os.Stderr, "File Error: %s\n", err)
              return
       }

       fmt.Printf("%s\n", string(buf))
       err = ioutil.WriteFile(outputFile, buf, 0x644)
       if err != nil {
              panic(err.Error())
       }
}
```


```
//读取压缩文件示例
package main

import (
       "bufio"
       "compress/gzip"
       "fmt"
       "os"
)
func main() {
       fName := "MyFile.gz"
       var r *bufio.Reader
       fi, err := os.Open(fName)
       if err != nil {
              fmt.Fprintf(os.Stderr, "%v, Can’t open %s: error: %s\n", os.Args[0], fName, err)
              os.Exit(1)
       }
       fz, err := gzip.NewReader(fi)
       if err != nil {
              fmt.Fprintf(os.Stderr, "open gzip failed, err: %v\n", err)
              return
       }
       r = bufio.NewReader(fz)
       for {
              line, err := r.ReadString('\n')
              if err != nil {
                     fmt.Println("Done reading file")
                     os.Exit(0)
              }
              fmt.Println(line)
       }
}
```


```
//文件写入
package main

import (
       "bufio"
       "fmt"
       "os"
)

func main() {
       outputFile, outputError := os.OpenFile("output.dat",
              os.O_WRONLY|os.O_CREATE, 0666)
       if outputError != nil {
              fmt.Printf("An error occurred with file creation\n")
              return
       }

       defer outputFile.Close()
       outputWriter := bufio.NewWriter(outputFile)
       outputString := "hello world!\n"
       for i := 0; i < 10; i++ {
              outputWriter.WriteString(outputString)
       }
       outputWriter.Flush()
}
```


```
//拷贝文件
package main

import (
       "fmt"
       "io"
       "os"
)

func main() {

       CopyFile("target.txt", "source.txt")
       fmt.Println("Copy done!")
}

func CopyFile(dstName, srcName string) (written int64, err error) {
       src, err := os.Open(srcName)
       if err != nil {
              return
       }
       defer src.Close()
       dst, err := os.OpenFile(dstName, os.O_WRONLY|os.O_CREATE, 0644)
       if err != nil {
              return
       }
       defer dst.Close()
       return io.Copy(dst, src)
}
```








