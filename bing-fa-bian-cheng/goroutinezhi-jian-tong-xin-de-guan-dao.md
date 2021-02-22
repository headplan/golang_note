# goroutine之间通信的管道

如果说 goroutine 是 Go语言程序的并发体的话 , 那么 channels 就是它们之间的通信机制 . 一个 channels 是一个通信机制 , 它可以让一个 goroutine 通过它给另一个 goroutine 发送值信息 . 每个 channel 都有一个特殊的类型 , 也就是 channels 可发送数据的类型 . 一个可以发送 int 类型数据的 channel 一般写为 chan int . 

Go语言提倡使用通信的方法代替共享内存 , 当一个资源需要在 goroutine 之间共享时 , 通道在 goroutine 之间架起了一个管道 , 并提供了确保同步交换数据的机制 . 声明通道时 , 需要指定将要被共享的数据的类型 . 可以通过通道共享内置类型、命名类型、结构类型和引用类型的值或者指针 . 

![](/assets/goroutineandchannel.png)

  




