# 关于

官网 : [https://golang.org/](https://golang.org/) 或 [https://golang.google.cn/](https://golang.google.cn/)

**中文社区**

[https://studygolang.com/](https://studygolang.com/)

[http://www.runoob.com/go/go-tutorial.html](http://www.runoob.com/go/go-tutorial.html)

**在线运行**

[https://play.golang.org/](https://play.golang.org/)

**相关书籍**

&lt;Go程序设计语言&gt;

&lt;Go并发编程实战&gt;

**Go语言标准库**

[https://books.studygolang.com/The-Golang-Standard-Library-by-Example/](https://books.studygolang.com/The-Golang-Standard-Library-by-Example/)

**Go语言规范文档**

[https://golang.google.cn/ref/spec](https://golang.google.cn/ref/spec)

#### 介绍

Go语言初步设想始于2007年 , 主要是为了解决Google在软件开发中面临的问题 :

* 多核硬件架构 ; 
* 超大规模分布式计算集群 ; 
* Web 开发模式导致的前所未有的开发规模和更新速度 . 

Go的三位创始人 :

**Rob Pike**

Unix 的早期开发者

UTF-8 创始人

**Ken Thompson**

Unix 的创始人

C 语言创始人

1983 年获图灵奖

**Robert Griesemer**

Google V8 JS Engine 开发者

Hot Spot 开发者

Go的语言特性 :

**简单** : Go只有25个关键字 , C有37个 , C++ 11有84个 . 对于一些复杂编程任务如 : 并发编程 , 内存管理 , Go语言有内置的并发支持及垃圾回收机制 .

**高效** : Go是编译的静态类型语言 , 尽管支持了垃圾回收 , 但GO中仍可以通过指针进行直接内存访问 .

**生产力** : Go语言有简单清晰的依赖管理 , 简洁的语法 , 以及独特的接口类型 , 甚至是一些编程方式的约束 . 例如 , 支持复合而不是继承的扩展方式 .

切换到Go语言常常陷入的误区 :

* 大量使用共享内存的方式进行并发控制 , 而忽略了 Go 内置的CSP并发机制
* Java程序员在编写Go程序喜欢在方法调用间直接传递数组 , 导致大量内存复制 . 与Java不同 , Go的数组参数是通过值复制来传递的
* Java 程序员用Go时也总是喜欢创建一个只包含接口定义的包 , 以处理依赖关系 . 而在Go中其实大可不必 , 在 Go 中接口的实现对接口定义是没有依赖的



