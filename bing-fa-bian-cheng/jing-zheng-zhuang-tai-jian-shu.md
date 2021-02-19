# 竞争状态简述

有并发就有资源竞争 , 如果两个或者多个 goroutine 在没有相互同步的情况下 , 访问某个共享的资源 , 比如同时对该资源进行读写时 , 就会处于相互竞争的状态 , 这就是并发中的资源竞争 .

并发本身并不复杂 , 但是因为有了资源竞争的问题 , 就使得我们开发出好的并发程序变得复杂起来 , 因为会引起很多莫名其妙的问题 .

下面的代码中就会出现竞争状态 :

```go
package main

import (
    "fmt"
    "runtime"
    "sync"
)

var (
    count int32
    wg sync.WaitGroup
)

func main() {
    wg.Add(2)
    go incCount()
    go incCount()
    wg.Wait()
    fmt.Println(count)
}

func incCount() {
    defer wg.Done()
    for i := 0; i < 2; i++ {
        value := count
        runtime.Gosched()
        value++
        count = value
    }
}
```

这是一个资源竞争的例子 , 可以将程序多运行几次 , 会发现结果可能是 2 , 也可以是 3 , 还可能是 4 . 这是因为 count 变量没有任何同步保护 , 所以两个 goroutine 都会对其进行读写 , 会导致对已经计算好的结果被覆盖 , 以至于产生错误结果 . 

> 代码中的 runtime.Gosched\(\) 是让当前 goroutine 暂停的意思 , 退回执行队列 , 让其他等待的 goroutine 运行 , 目的是为了使资源竞争的结果更明显 .

通过上面的分析可以看出 , 之所以出现上面的问题 , 是因为两个 goroutine 相互覆盖结果 . 所以对于同一个资源的读写必须是原子化的 , 也就是说 , 同一时间只能允许有一个 goroutine 对共享资源进行读写操作 . 

共享资源竞争的问题 , 非常复杂 , 并且难以察觉 , 好在 Go 为我们提供了一个工具帮助我们检查 , 这个就是`go build -race`命令 . 在项目目录下执行这个命令 , 生成一个可以执行文件 , 然后再运行这个可执行文件 , 就可以看到打印出的检测信息 . 

```
==================
WARNING: DATA RACE
Read at 0x000001274c98 by goroutine 8:
  main.incCount()
      /test.go:25 +0x7c

Previous write at 0x000001274c98 by goroutine 7:
  main.incCount()
      /test.go:28 +0x9b

Goroutine 8 (running) created at:
  main.main()
      /test.go:17 +0x7c

Goroutine 7 (finished) created at:
  main.main()
      /test.go:16 +0x64
==================
4
Found 1 data race(s)
```



