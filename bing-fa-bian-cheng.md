# 并发编程

并发指在同一时间内可以执行多个任务 . 并发编程含义比较广泛 , 包含多线程编程、多进程编程及分布式程序等 . 这里并发含义属于多线程编程 .

Go 语言通过编译器运行时\(runtime\) , 从语言上支持了并发的特性 . Go语言的并发通过**goroutine**特性完成 . goroutine类似于线程 , 但是可以根据需要创建多个goroutine并发工作 . goroutine是由Go语言的运行时调度完成 , 而线程是由操作系统调度完成 .

Go 语言还提供**channel**在多个goroutine间进行通信 . goroutine和channel是Go语言秉承的CSP\(Communicating Sequential Process\)并发模式的重要实现基础 . 

