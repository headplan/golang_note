# 错误处理与测试

Go语言的错误处理思想及设计包含以下特征 :

* 一个可能造成错误的函数 , 需要返回值中返回一个错误接口\(error\) , 如果调用是成功的 , 错误接口将返回 nil , 否则返回错误 . 
* 在函数调用后需要检查错误 , 如果发生错误 , 则进行必要的错误处理 . 

Go语言没有类似Java或 .NET 中的异常处理机制 , 虽然可以使用 defer、panic、recover 模拟 , 但官方并不主张这样做 . Go语言的设计者认为其他语言的异常机制已被过度使用 , 上层逻辑需要为函数发生的异常付出太多的资源 . 同时 , 如果函数使用者觉得错误处理很麻烦而忽略错误 , 那么程序将在不可预知的时刻崩溃 .

Go语言希望开发者将错误处理视为正常开发必须实现的环节 , 正确地处理每一个可能发生错误的函数 . 同时 , Go语言使用返回值返回错误的机制 , 也能大幅降低编译器、运行时处理错误的复杂度 , 让开发者真正地掌握错误的处理 .

#### net 包中的例子

net.Dial\(\) 是Go语言系统包 net 即中的一个函数 , 一般用于创建一个 Socket 连接 .

net.Dial 拥有两个返回值 , 即 Conn 和 error . 这个函数是阻塞的 , 因此在 Socket 操作后 , 会返回 Conn 连接对象和 error , 如果发生错误 , error 会告知错误的类型 , Conn 会返回空 .

根据Go语言的错误处理机制 , Conn 是其重要的返回值 , 因此 , 为这个函数增加一个错误返回 , 类似为 error :

```go
func Dial(network, address string) (Conn, error) {
    var d Dialer
    return d.Dial(network, address)
}
```

在 io 包中的 Writer 接口也拥有错误返回 :

```go
type Writer interface {
    Write(p []byte) (n int, err error)
}
```

io 包中还有 Closer 接口 , 只有一个错误返回 : 

```go
type Closer interface {
    Close() error
}
```

#### 错误接口的定义格式

error 是 Go 系统声明的接口类型 : 

```go
type error interface {
    Error() string
}
```

所有符合 Error\(\)string 格式的方法 , 都能实现错误接口 , Error\(\) 方法返回错误的具体描述 , 使用者可以通过这个字符串知道发生了什么错误 . 

#### 自定义一个错误

返回错误前 , 需要定义会产生哪些可能的错误 , 在Go语言中 , 使用 errors 包进行错误的定义 : 

```go
var err = errors.New("this is an error")
```



