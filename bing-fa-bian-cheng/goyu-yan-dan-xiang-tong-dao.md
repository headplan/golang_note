# Go语言单向通道

Go语言的类型系统提供了单方向的 channel 类型 , 顾名思义 , 单向 channel 就是只能用于写入或者只能用于读取数据 . 当然 channel 本身必然是同时支持读写的 , 否则根本没法用 .

假如一个 channel 真的只能读取数据 , 那么它肯定只会是空的 , 因为你没机会往里面写数据 . 同理 , 如果一个 channel 只允许写入数据 , 即使写进去了 , 也没有丝毫意义 , 因为没有办法读取到里面的数据 . 所谓的单向 channel 概念 , 其实只是对 channel 的一种使用限制 .

#### 单向通道的声明格式

我们在将一个 channel 变量传递到一个函数时 , 可以通过将其指定为单向 channel 变量 , 从而限制该函数中可以对此 channel 的操作 , 比如只能往这个 channel 中写入数据 , 或者只能从这个 channel 读取数据 .

单向 channel 变量的声明非常简单 , 只能写入数据的通道类型为`chan<-` , 只能读取数据的通道类型为`<-chan` , 格式如下 :

```go
var 通道实例 chan<- 元素类型 // 只能写入数据的通道
var 通道实例 <-chan 元素类型 // 只能读取数据的通道
```

* 元素类型 : 通道包含的元素类型 . 
* 通道实例 : 声明的通道变量 . 

#### 单向通道的使用例子

```
ch := make(chan int)
// 声明一个只能写入数据的通道类型,并赋值为ch
var chSendOnly chan<- int = ch
//声明一个只能读取数据的通道类型,并赋值为ch
var chRecvOnly <-chan int = ch
```

使用 make 创建通道时 , 也可以创建一个只写入或只读取的通道 :

```go
ch := make(<-chan int)
var chReadOnly <-chan int = ch
<-chReadOnly
```

上面代码编译正常 , 运行也是正确的 . 但是 , 一个不能写入数据只能读取的通道是毫无意义的 . 

#### time包中的单向通道

time 包中的计时器会返回一个 timer 实例 , 代码如下 : 

```go
timer := time.NewTimer(time.Second)
```



