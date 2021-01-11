# 命令源码文件

环境变量 GOPATH 指向的是一个或多个工作区 , 每个工作区中都会有以代码包为基本组织形式的源码文件 .

这里的源码文件又分为三种 :

* 命令源码文件
* 库源码文件
* 测试源码文件

它们都有着不同的用途和编写规则 .

#### 命令源码文件

* 独立程序的入口
* 属于main包 , 包含无参数无结果的main函数
* 可通过`go run`命令运行 , 可接受命令行参数
* main函数执行的结果就意味着当前程序运行的结束
* 同一个代码包中不要放多个命令源码文件
* 命令源码文件与库源码文件也不要放在同一个代码包
* 构建
  * 构建后生成可执行文件
    * 可执行文件即executable file
      * 可在命令行中运行的文件
      * 在Windows中就是扩展名为`.exe`的文件
      * 在Linux中的一般无扩展名
  * 生成位置在命令执行目录
* 安装
  * 安装后生成可执行文件
  * 生成位置在当前工作区的bin子目录或GOBIN包含的目录

#### 命令源码文件的用途及编写

命令源码文件是程序的运行入口 , 是每个可独立运行的程序必须拥有的 . 可以通过构建或安装 , 生成与其对应的可执行文件 , 后者一般会与该命令源码文件的直接父目录同名 .

如果一个源码文件声明属于main包 , 并且包含一个无参数声明且无结果声明的main函数 , 那么它就是命令源码文件 .

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Golang!")
}
```

> 当需要模块化编程时 , 往往会将代码拆分到多个文件 , 甚至拆分到不同的代码包中 . 但无论怎样 , 对于一个独立的程序来说 , 命令源码文件永远只会也只能有一个 . 如果有与命令源码文件同包的源码文件 , 那么它们也应该声明属于main包 .

#### 命令参数的接收和解析

##### **命令源码文件怎样接收参数**

go语言中的flag用于接收和解析命令参数 .

首先 , 在代码中使用某个包中的程序实体 , 那么应该先导入这个包 .

```go
package main

import (
    "flag"
    "fmt"
)

func main() {
    fmt.Println("Hello, Golang!")
}
```

然后声明人名的变量 , 人名肯定是由字符串代表的 , 然后调用flag包的StringVar函数的代码 :

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

var name string

func init() {
    flag.StringVar(&name, "name", "everyone", "set your name")
}

func main() {
    fmt.Printf("Hello,%s!\n", name)
    os.Exit(0)
}
```

函数flag.StringVar接受4个参数

* 第 1 个参数是用于存储该命令参数值的地址 , 具体到这里就是在前面声明的变量name的地址了 , 由表达式&name表示
* 第 2 个参数是为了指定该命令参数的名称 , 这里是name . 
* 第 3 个参数是为了指定在未追加该命令参数时的默认值 , 这里是everyone . 
* 第 4 个函数参数 , 即是该命令参数的简短说明了 , 这在打印命令说明时会用到 . 

> 还有一个与flag.StringVar函数类似的函数 , 叫flag.String . 这两个函数的区别是 , 后者会直接返回一个已经分配好的用于存储命令参数值的地址 .

再说最后一步 . 需要添加代码`flag.Parse()` . 函数flag.Parse用于真正解析命令参数 , 并把它们的值赋给相应的变量 . 

对该函数的调用必须在所有命令参数存储载体的声明和设置之后 , 并且在读取任何命令参数值之前进行 . 

```
package main

import (
    "flag"
    "fmt"
    "os"
)

var name string

func init() {
    flag.StringVar(&name, "name", "everyone", "set your name")
}

func main() {
    flag.Parse()
    fmt.Printf("Hello,%s!\n", name)
    os.Exit(0)
}
```



