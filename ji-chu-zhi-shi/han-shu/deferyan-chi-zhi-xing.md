# defer延迟执行

Go语言的 defer 语句会将其后面跟随的语句进行延迟处理 , 在 defer 归属的函数即将返回时 , 将延迟处理的语句按 defer 的逆序进行执行 , 也就是说先被 defer 的语句最后被执行 , 最后被 defer 的语句 , 最先被执行 .

关键字 defer 的用法类似于面向对象编程语言Java和C\#的finally语句块 , 它一般用于释放某些已分配的资源 , 典型的例子就是对一个互斥解锁或者关闭一个文件 .

#### 多个延迟执行语句的处理顺序

当有多个 defer 行为被注册时 , 它们会以逆序执行 . 类似栈 , 即后进先出 :

```go
package main

import (
    "fmt"
)

func main() {

    fmt.Println("defer begin")

    // 将defer放入延迟调用栈
    defer fmt.Println(1)

    defer fmt.Println(2)

    // 最后一个放入, 位于栈顶, 最先调用
    defer fmt.Println(3)

    fmt.Println("defer end")
}
```

延迟调用是在 defer 所在函数结束时进行 , 函数结束可以是正常返回时 , 也可以是发生宕机时 .

#### 使用延迟执行语句在函数退出时释放资源

处理业务或逻辑中涉及成对的操作是一件比较烦琐的事情 , 比如打开和关闭文件、接收请求和回复请求、加锁和解锁等 . 在这些操作中 , 最容易忽略的就是在每个函数退出处正确地释放和关闭资源 .

defer 语句正好是在函数退出时执行的语句 , 所以使用 defer 能非常方便地处理资源释放问题 .

#### 使用延迟并发解锁

在下面的例子中会在函数中并发使用 map , 为防止竞态问题 , 使用 sync.Mutex 进行加锁 :

```go
package main

import "sync"

var (
    // 声明一个map
    valueByKey = make(map[string]int)
    // 保证使用map时的并发安全的互斥锁
    valueByKeyGuard sync.Mutex
)

func readValue(key string) int {
    // 对共享资源加锁
    valueByKeyGuard.Lock()
    // 取值
    v := valueByKey[key]
    // 对共享资源解锁
    valueByKeyGuard.Unlock()

    return v
}
```

使用 defer 语句对上面的语句进行简化 :

```go
package main

import "sync"

var (
    // 声明一个map
    valueByKey = make(map[string]int)
    // 保证使用map时的并发安全的互斥锁
    valueByKeyGuard sync.Mutex
)

func readValue(key string) int {
    // 对共享资源加锁
    valueByKeyGuard.Lock()
    // 延迟到函数结束时调用,对共享资源解锁
    defer valueByKeyGuard.Unlock()

    return valueByKey[key]
}
```

#### 使用延迟释放文件句柄

文件的操作需要经过打开文件、获取和操作文件资源、关闭资源几个过程 , 如果在操作完毕后不关闭文件资源 , 进程将一直无法释放文件资源 , 在下面的例子中将实现根据文件名获取文件大小的函数 , 函数中需要打开文件、获取文件大小和关闭文件等操作 , 由于每一步系统操作都需要进行错误处理 , 而每一步处理都会造成一次可能的退出 , 因此就需要在退出时释放资源 , 而我们需要密切关注在函数退出处正确地释放文件资源 , 参考下面的代码 : 

```go
package main

import "os"

func fileSize(filename string) int64 {
	// 根据文件名打开文件,返回文件句柄和错误
	f, err := os.Open(filename)

	// 如果打开时发生错误,返回文件大小为0
	if err != nil {
		return 0
	}

	// 取文件状态信息
	info, err := f.Stat()
	
	// 如果获取信息时发生错误,关闭文件并返回文件大小为0
	if err != nil {
		f.Close()
		return 0
	}
	
	// 取文件大小
	size := info.Size()
	
	// 关闭文件
	f.Close()
	
	// 返回文件大小
	return size
}

```



