# 通道实现并发打印

前面的例子创建的都是无缓冲通道 . 使用无缓冲通道往里面装入数据时 , 装入方将被阻塞 , 直到另外通道在另外一个 goroutine 中被取出 . 同样 , 如果通道中没有放入任何数据 , 接收方试图从通道中获取数据时 , 同样也是阻塞 . 发送和接收的操作是同步完成的 . 

```go
package main

import (
	"fmt"
)

func printer(c chan int) {
	// 开始无限循环等待数据
	for {
		// 从channel中获取一个数据
		data := <-c
		// 将0视为数据结束
		if data == 0 {
			break
		}
		// 打印数据
		fmt.Println(data)
	}
	// 通知main已经结束循环(我搞定了!)
	c <- 0
}

func main() {
	// 创建一个channel
	c := make(chan int)
	// 并发执行printer,传入channel
	go printer(c)

	for i := 1; i <= 10; i++ {
		// 将数据通过channel投送给printer
		c <- i
	}
	// 通知并发的printer结束循环(没数据啦!)
	c <- 0
	// 等待printer结束(搞定喊我!)
	<-c
}

```



