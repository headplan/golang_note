# Go语言无缓冲的通道

Go语言中无缓冲的通道\(unbuffered channel\)是指在接收前没有能力保存任何值的通道 . 这种类型的通道要求发送 goroutine 和接收 goroutine 同时准备好 , 才能完成发送和接收操作 .

如果两个 goroutine 没有同时准备好 , 通道会导致先执行发送或接收操作的 goroutine 阻塞等待 . 这种对通道进行发送和接收的交互行为本身就是同步的 . 其中任意一个操作都无法离开另一个操作单独存在 .

阻塞指的是由于某种原因数据没有到达 , 当前协程\(线程\)持续处于等待状态 , 直到条件满足才解除阻塞 .

同步指的是在两个或多个协程\(线程\)之间 , 保持数据内容一致性的机制 .

下图展示两个 goroutine 如何利用无缓冲的通道来共享一个值 .

![](/assets/wuhuanchong.png)

在第 1 步 , 两个 goroutine 都到达通道 , 但哪个都没有开始执行发送或者接收 .

在第 2 步 , 左侧的 goroutine 将它的手伸进了通道 , 这模拟了向通道发送数据的行为 . 这时 , 这个 goroutine 会在通道中被锁住 , 直到交换完成 .

在第 3 步 , 右侧的 goroutine 将它的手放入通道 , 这模拟了从通道里接收数据 . 这个 goroutine 一样也会在通道中被锁住 , 直到交换完成 .

在第 4 步和第 5 步 , 进行交换 , 并最终在第 6 步 , 两个 goroutine 都将它们的手从通道里拿出来 , 这模拟了被锁住的 goroutine 得到释放 . 两个 goroutine 现在都可以去做别的事情了 .

使用两个 goroutine 来模拟网球比赛 , 并使用无缓冲的通道来模拟球的来回 :

```go
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

// wg 用来等待程序结束
var wg sync.WaitGroup

func init() {
    rand.Seed(time.Now().UnixNano())
}

func main() {
    // 创建一个无缓冲的通道
    court := make(chan int)

    // 计数加2,表示要等待两个goroutine
    wg.Add(2)

    // 启动两个选手
    go player("Nadal", court)
    go player("Djokovic", court)

    // 发球
    court <- 1
    // 等待游戏结束
    wg.Wait()
}

func player(name string, court chan int) {
    // 在函数退出时调用Done,来通知main函数工作已经完成
    defer wg.Done()

    for {
        // 等待球被击打过来
        ball, ok := <-court
        if !ok {
            // 如果通道被关闭,我们就赢了
            fmt.Printf("Player %s Won\n", name)
            return
        }

        // 选随机数,然后用这个数来判断我们是否丢球
        n := rand.Intn(100)
        if n%13 == 0 {
            fmt.Printf("Player %s Missed\n", name)
            // 关闭通道,表示我们输了
            close(court)
            return
        }
        // 显示击球数,并将击球数加1
        fmt.Printf("Player %s Hit %d\n", name, ball)
        ball++
        // 将球打向对手
        court <- ball
    }
}
```

代码说明如下 : 

* 第 22 行 , 创建了一个 int 类型的无缓冲的通道 , 让两个 goroutine 在击球时能够互相同步 . 
* 第 28 行和第 29 行 , 创建了参与比赛的两个 goroutine . 在这个时候 , 两个 goroutine 都阻塞住等待击球 . 
* 第 32 行 , 将球发到通道里 , 程序开始执行这个比赛，直到某个 goroutine 输掉比赛。
* 第 43 行可以找到一个无限循环的 for 语句。在这个循环里，是玩游戏的过程。
* 第 45 行，goroutine 从通道接收数据，用来表示等待接球。这个接收动作会锁住 goroutine，直到有数据发送到通道里。通道的接收动作返回时。
* 第 46 行会检测 ok 标志是否为 false。如果这个值是 false，表示通道已经被关闭，游戏结束。
* 第 53 行到第 60 行，会产生一个随机数，用来决定 goroutine 是否击中了球。
* 第 58 行如果某个 goroutine 没有打中球，关闭通道。之后两个 goroutine 都会返回，通过 defer 声明的 Done 会被执行，程序终止。
* 第 64 行，如果击中了球 ball 的值会递增 1，并在第 67 行，将 ball 作为球重新放入通道，发送给另一位选手。在这个时刻，两个 goroutine 都会被锁住，直到交换完成。

用不同的模式 , 使用无缓冲的通道 , 在 goroutine 之间同步数据 , 来模拟接力比赛 :

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

// 等待程序结束
var wg sync.WaitGroup

func main() {
    // 创建一个无缓冲通道
    baton := make(chan int)

    // 给最后一位跑步者将计数加1
    wg.Add(1)

    // 第一位跑步者持有接力棒
    go Runner(baton)

    // 开始比赛
    baton <- 1

    // 等待比赛结束
    wg.Wait()
}

func Runner(baton chan int) {
    var newRunner int

    // 等待接力棒
    runner := <-baton

    // 开始跑步
    fmt.Printf("Runner %d Running With Baton\n", runner)

    // 创建下一位跑步者
    if runner != 4 {
        newRunner = runner + 1
        fmt.Printf("Runner %d To The Line\n", newRunner)
        go Runner(baton)
    }

    // 接力跑时间
    time.Sleep(1000 * time.Millisecond)

    // 判断比赛是否结束
    if runner == 4 {
        fmt.Printf("Runner %d Finished, Race Over\n", runner)
        wg.Done()
        return
    }

    // 将接力棒交给下一位跑步者
    fmt.Printf("Runner %d Exchange With Runner %d\n", runner, newRunner)

    baton <- newRunner
}
```



