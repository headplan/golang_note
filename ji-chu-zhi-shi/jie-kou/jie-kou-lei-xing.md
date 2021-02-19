# 接口类型

接口类型具体描述了一系列方法的集合 , 一个实现了这些方法的具体类型是这个接口类型的实例 .

io.Writer类型是用的最广泛的接口之一 , 因为它提供了所有的类型写入bytes的抽象 , 包括文件类型 , 内存缓冲区 , 网络链接 , HTTP客户端 , 压缩工具 , 哈希等等 . io包中定义了很多其它有用的接口类型 . Reader可以代表任意可以读取bytes的类型 , Closer可以是任意可以关闭的值 , 例如一个文件或是网络链接 .

```go
package io 
type Reader interface { 
    Read(p []byte) (n int, err error)
}
type Closer interface {
    Close() error
}
```

在往下看 , 我们发现有些新的接口类型通过组合已经有的接口来定义 .

```
type ReadWriter interface { 
    Reader 
    Writer 
}
type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

上面用到的语法和结构内嵌相似 , 我们可以用这种方式以一个简写命名另一个接口 , 而不用声明它所有的方法 . 这种方式本称为接口内嵌 . 尽管略失简洁 , 我们可以像下面这样 , 不使用内嵌来声明io.Writer接口 .

```go
type ReadWriter interface {
    Read(p []byte) (n int, err error) 
    Write(p []byte) (n int, err error) 
}
```

或者使用混合的风格 :

```go
type ReadWriter interface { 
    Read(p []byte) (n int, err error) 
    Writer
}
```

上面3种定义方式都是一样的效果 . 方法的顺序变化也没有影响 , 唯一重要的就是这个集合里面的方法 . 
