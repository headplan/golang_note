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
	c <- 99
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
	aa := <-c
	fmt.Println(aa)

}
```

代码说明如下

* 第 10 行 , 创建一个无限循环 , 只有当第 16 行获取到的数据为 0 时才会退出循环 . 
* 第 13 行 , 从函数参数传入的通道中获取一个整型数值 . 
* 第 21 行 , 打印整型数值 . 
* 第 25 行 , 在退出循环时 , 通过通道通知 main\(\) 函数已经完成工作 . 
* 第 32 行 , 创建一个整型通道进行跨 goroutine 的通信 . 
* 第 35 行 , 创建一个 goroutine , 并发执行 printer\(\) 函数 . 
* 第 37 行 , 构建一个数值循环 , 将 1～10 的数通过通道传送给 printer 构造出的 goroutine . 
* 第 44 行 , 给通道传入一个 0 , 表示将前面的数据处理完成后 , 退出循环 . 
* 第 47 行 , 在数据发送过去后 , 因为并发和调度的原因 , 任务会并发执行 . 这里需要等待 printer 的第 25 行返回数据后 , 才可以退出 main\(\) . 



