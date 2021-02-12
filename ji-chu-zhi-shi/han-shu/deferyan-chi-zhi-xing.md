# defer延迟执行

Go语言的 defer 语句会将其后面跟随的语句进行延迟处理 , 在 defer 归属的函数即将返回时 , 将延迟处理的语句按 defer 的逆序进行执行 , 也就是说先被 defer 的语句最后被执行 , 最后被 defer 的语句 , 最先被执行 . 

关键字 defer 的用法类似于面向对象编程语言Java和C\#的finally语句块 , 它一般用于释放某些已分配的资源 , 典型的例子就是对一个互斥解锁或者关闭一个文件 . 

#### 多个延迟执行语句的处理顺序

当有多个 defer 行为被注册时 , 它们会以逆序执行 . 类似栈 , 即后进先出 : 

```go
package main

import (
    "fmt"
)

func main() {

    fmt.Println("defer begin")

    // 将defer放入延迟调用栈
    defer fmt.Println(1)

    defer fmt.Println(2)

    // 最后一个放入, 位于栈顶, 最先调用
    defer fmt.Println(3)

    fmt.Println("defer end")
}
```



