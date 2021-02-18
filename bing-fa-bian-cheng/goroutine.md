# goroutine

在编写 Socket 网络程序时 , 需要提前准备一个线程池为每一个 Socket 的收发包分配一个线程 . 开发人员需要在线程数量和 CPU 数量间建立一个对应关系 , 以保证每个任务能及时地被分配到 CPU 上进行处理 , 同时避免多个任务频繁地在线程间切换执行而损失效率 .

虽然 , 线程池为逻辑编写者提供了线程分配的抽象机制 . 但是 , 如果面对随时随地可能发生的并发和线程处理需求 , 线程池就不是非常直观和方便了 . 能否有一种机制 : 使用者分配足够多的任务 , 系统能自动帮助使用者把任务分配到 CPU 上 , 让这些任务尽量并发运作 . 这种机制在 Go语言中被称为 **goroutine . **

goroutine 是 Go语言中的轻量级线程实现 , 由 Go 运行时\(runtime\)管理 . Go程序会智能地将 goroutine 中的任务合理地分配给每个CPU .

Go 程序从 main 包的 main\(\) 函数开始 , 在程序启动时 , Go 程序就会为 main\(\) 函数创建一个默认的 goroutine .

#### 使用普通函数创建 goroutine

Go 程序中使用 **go** 关键字为一个函数创建一个 goroutine . 一个函数可以被创建多个 goroutine , 一个 goroutine 必定对应一个函数 .

##### 格式

为一个普通函数创建 goroutine 的写法如下 :

```
go 函数名(参数列表)
```

* 函数名 : 要调用的函数名 . 
* 参数列表 : 调用函数需要传入的参数 . 

使用 go 关键字创建 goroutine 时 , 被调用函数的返回值会被忽略 .

> 如果需要在 goroutine 中返回数据 , 需要使用通道\(channel\)特性 , 通过通道把数据从 goroutine 中作为返回值传出 .

##### 例子

使用 go 关键字 , 将 running\(\) 函数并发执行 , 每隔一秒打印一次计数器 , 而 main 的 goroutine 则等待用户输入 , 两个行为可以同时进行 .

```go
package main

import (
    "fmt"
    "time"
)

func running() {
    var times int
    for {
        times++
        fmt.Println("tick", times)
        time.Sleep(time.Second)
    }
}

func main() {
    go running()

    var input string
    fmt.Scanln(&input)
}
```

代码说明如下 : 

* 第 12 行 , 使用 for 形成一个无限循环 . 
* 第 13 行 , times 变量在循环中不断自增 . 
* 第 14 行 , 输出 times 变量的值 . 
* 第 17 行 , 使用 time.Sleep 暂停 1 秒后继续循环 . 
* 第 25 行 , 使用 go 关键字让 running\(\) 函数并发运行 . 
* 第 29 行 , 接受用户输入 , 直到按 Enter 键时将输入的内容写入 input 变量中并返回 , 整个程序终止 . 

![](/assets/goroutine.png)



