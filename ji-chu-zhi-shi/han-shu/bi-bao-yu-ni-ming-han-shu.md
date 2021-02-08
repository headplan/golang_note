# 闭包与匿名函数

#### 把函数作为值保存到变量中

在Go语言中 , 函数也是一种类型 , 可以和其他类型一样保存在变量中 , 下面的代码定义了一个函数变量 f , 并将一个函数名为 fire\(\) 的函数赋给函数变量 f , 这样调用函数变量 f 时 , 实际调用的就是 fire\(\) 函数 .

```go
package main
import (
    "fmt"
)
func fire() {
    fmt.Println("fire")
}
func main() {
    var f func()
    f = fire
    f()
}
```



