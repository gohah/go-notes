# go 实现一个负载均衡调度算法，支持随机、轮训等算法
> balance包：


```
//定义负载均衡接口
package balance

type Balancer interface {
       DoBalance([]*Instance, ...string) (*Instance, error)
}
```


```
//定义服务器实例类
package balance

import "strconv"

type Instance struct {
       host string
       port int
}

func NewInstance(host string, port int) *Instance {
       return &Instance{
              host: host,
              port: port,
       }
}

func (p *Instance) GetHost() string {
       return p.host
}

func (p *Instance) GetPort() int {
       return p.port
}

func (p *Instance) String() string {
       return p.host + ":" + strconv.Itoa(p.port)
}
```


```
//定义随机模式
package balance

import (
       "errors"
       "math/rand"
)

func init() {
       RegisterBalancer("random", &RandomBalance{})
}

type RandomBalance struct {
}

func (p *RandomBalance) DoBalance(insts []*Instance, key ...string) (inst *Instance, err error) {

       if len(insts) == 0 {
              err = errors.New("No instance")
              return
       }

       lens := len(insts)
       index := rand.Intn(lens)
       inst = insts[index]

       return
}
```


```
//定义轮询模式
package balance

import (
       "errors"
)

func init() {
       RegisterBalancer("roundrobin", &RoundRobinBalance{})
}

type RoundRobinBalance struct {
       curIndex int
}

func (p *RoundRobinBalance) DoBalance(insts []*Instance, key ...string) (inst *Instance, err error) {

       if len(insts) == 0 {
              err = errors.New("No instance")
              return
       }

       lens := len(insts)
       if p.curIndex >= lens {
              p.curIndex = 0
       }

       inst = insts[p.curIndex]
       p.curIndex = (p.curIndex + 1) % lens
       return
}
```


```
//定义管理负载均衡类
package balance

import "fmt"

type BalanceMgr struct {
       allBalancer map[string]Balancer
}

var mgr = BalanceMgr{
       allBalancer: make(map[string]Balancer),
}

func (p *BalanceMgr) registerBalancer(name string, b Balancer) {
       p.allBalancer[name] = b
}

func RegisterBalancer(name string, b Balancer) {
       mgr.registerBalancer(name, b)
}

func DoBalance(name string, insts []*Instance) (inst *Instance, err error) {
       balancer, ok := mgr.allBalancer[name]
       if !ok {
              err = fmt.Errorf("Not found %s balancer", name)
              return
       }

       fmt.Printf("use %s balancer\n", name)
       inst, err = balancer.DoBalance(insts)
       return
}
```

> main包


```
//自定义hash模式
package main

import (
       "fmt"
       "hash/crc32"
       "math/rand"
       "work/eg1/balance"
)

type HashBalance struct {
}

func init() {
       balance.RegisterBalancer("hash", &HashBalance{})
}

func (p *HashBalance) DoBalance(insts []*balance.Instance, key ...string) (inst *balance.Instance, err error) {
       var defKey string = fmt.Sprintf("%d", rand.Int())
       if len(key) > 0 {
              defKey = key[0]
       }

       lens := len(insts)
       if lens == 0 {
              err = fmt.Errorf("No backend instance")
              return
       }

       crcTable := crc32.MakeTable(crc32.IEEE)
       hashVal := crc32.Checksum([]byte(defKey), crcTable)
       index := int(hashVal) % lens
       inst = insts[index]

       return
}
```


```
//模拟请求调用
package main

import (
       "fmt"
       "work/eg1/balance"
       "math/rand"
       "os"
       "time"
)

func main() {

       var insts []*balance.Instance
       for i := 0; i < 16; i++ {
              host := fmt.Sprintf("192.168.%d.%d", rand.Intn(255), rand.Intn(255))
              one := balance.NewInstance(host, 8080)
              insts = append(insts, one)
       }

       var balanceName = "random"
       if len(os.Args) > 1 {
              balanceName = os.Args[1]
       }

       for {
              inst, err := balance.DoBalance(balanceName, insts)
              if err != nil {
                     //fmt.Println("do balance err:", err)
                     fmt.Fprintf(os.Stdout, "do balance err\n")
                     continue
              }
              fmt.Println(inst)
              time.Sleep(time.Second)
       }

}
```



