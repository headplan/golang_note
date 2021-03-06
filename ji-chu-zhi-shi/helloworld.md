# Hello World

### 编写第一个Go程序

#### **开发环境构建**

**GOPATH**

在1.8版本前必须设置这个环境变量

1.8版本后\(包括1.8\)如果没有设置 , 则使用默认值

在 Unix 上默认为$HOME/go

在 Windows 上默认为%USERPROFILE%/go

**目录结构**

src - 源码路径 , GOPATH环境下的命名规则

#### 语言结构

**基础组成部分**

* 包声明 : 表明代码所在模块
* 引入包 : 代码依赖
* 函数 : 功能实现
* 变量
* 语句&表达式
* 注释

```go
package main

import "fmt"

func main() {
    /* 这是我的第一个简单的程序 */
    fmt.Println("hello,world")
}
```

* **第一行代码package main定义了包名\(必须\)** . 你必须在源文件中非注释的第一行指明这个文件属于哪个包 , 如package main . package main表示一个可独立执行的程序 , 每个Go应用程序都包含一个名为main的包 . 
  * **目录名不一定是main**
* 下一行`import "fmt"`告诉Go编译器这个程序需要使用fmt包的函数或者其他元素 , fmt包实现了格式化IO\(输入输出\)的函数 . 
* **下一行func main\(\)是程序开始执行的函数\(必须\)** . main函数是每一个可执行程序必须包含的 , 一般来说都是在启动后第一个执行的函数 , 当然如果有init\(\)函数会先执行init\(\)函数 . 
  * **文件名不一定是main**
* 下一行/\*...\*/是注释 , 在程序执行时将被忽略 . 单行注释用`//` , 多行注释用`/*...*/`
* `fmt.Println(...)`可以将字符串输出到控制台 , 并在最后自动增加换行字符`\n` 使用`fmt.Print("hello,world\n")`可以得到相同的结果 . `Print`和`Println`这两个函数也支持使用变量 , 没有其他指定默认打印变量输出到控制台 , 例如 , `fmt.Println(arr)`
* 当标识符以一个大写字母开头\(包括常量 , 变量 , 类型 , 函数名 , 结构字段等\) , 例如 , Group1 , 这种标识符的对象可以被外部包的代码使用 , 称为导出\(客户端程序需要先导入这个包\) . 有点像面向对象中的public , 标识符如果是小写开头的 , 则对包外是不可见的 . 在包内是可见并可用的 , 类似protected .

**执行Go程序**

```
go run helloworld.go
```

> 花括号"{"不能单独一行

**编译Go程序**

```
go build helloworld.go
```

> Go在默认情况下会使用静态连接 , 编译完的go程序是一个独立的二进制文件

**退出返回值**

* Go中main函数不支持任何返回值
* 通过os.Exit来返回状态

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    fmt.Println("hello world")
    os.Exit(0)
}
```

**获取命令行参数**

* main 函数不支持传入参数
* func main\(arg \[\]string\)

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    if len(os.Args) > 1 {
        fmt.Println("hello", os.Args[1])
    } else {
        fmt.Println("hello world")
    }
    os.Exit(0)
}
```



