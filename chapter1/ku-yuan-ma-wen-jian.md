# 库源码文件

#### 库源码文件

* 专用于放置可供具他代码使用的程序实体
* 构建
  * 作用在于检查和验证
  * 构建后只生成临时文件
    * 在操作系统的临时目录下
    * 开发者一般可以不用关心
* 安装
  * 安装后生成归档文件
    * 归档文件即archive file
      * 扩展名为.a的文件
      * 即为静态链接库文件
  * 生成位置在当前工作区的pkg子目录

库源码文件是不能被直接运行的源码文件 , 它仅用于存放程序实体 , 这些程序实体可以被其他代码使用 . 这里的"其他代码"可以与被使用的程序实体在同一个源码文件内 , 也可以在其他源码文件 , 甚至其他代码包中 .

> 在 Go 语言中 , 程序实体是变量、常量、函数、结构体和接口的统称 . 我们总是会先声明\(或者说定义\)程序实体 , 然后再去使用 . 比如 , 先定义了变量name , 然后在main函数中调用fmt.Printf函数的时候用到了它 .
>
> 程序实体的名字被统称为标识符 , 标识符可以是任何 Unicode 编码可以表示的字母字符、数字以及下划线“\_” , 但是其首字母不能是数字 .

#### 把命令源码文件中的代码拆分到其他库源码文件

新建一个命令源码文件

```go
package main

import "flag"

var myName string

func init() {
    flag.StringVar(&myName, "name", "", "set your name")
}
func main()  {
    flag.Parse()
    hello(myName)
}
```

这里的函数hello声明在另一个文件中 , 新建文件`hello_lib.go`

```go
package main

import "fmt"

func hello(name string) {
    fmt.Printf("Hello,%s!\n", name)
}
```

这里的package main是因为在同一个目录下的源码文件都需要被声明为属于同一个代码包 . 如果该目录下有一个命令源码文件 , 那么为了让同在一个目录下的文件都通过编译 , 其他源码文件应该也声明属于main包 .

现在就可以运行了 :

```
go run demo.go hello_lib.go --name="test"
```

像下面这样先构建当前的代码包再运行 :

```
go build ch1/demo
./demo --name="test"
```

#### 代码包声明的基本规则

第一条 - 同目录下的源码文件的代码包声明语句要一致 . 也就是说 , 它们要同属于一个代码包 . 这对于所有源码文件都是适用的 . 如果目录中有命令源码文件 , 那么其他种类的源码文件也应该声明属于main包 . 这也是我们能够成功构建和运行它们的前提 .

第二条 - 源码文件声明的代码包的名称可以与其所在的目录的名称不同 . 在针对代码包进行构建时 , 生成的结果文件的主名称与其父目录的名称一致 . 对于命令源码文件而言 , 构建生成的可执行文件的主名称会与其父目录的名称相同 .

#### 怎样把命令源码文件中的代码拆分到其他代码包

新建一个lib文件夹 , 在demo文件夹下 . 复制前面的文件 .

```
./demo
    ./libs
        ./demo_lib.go
    ./demo.go
```

代码包的拆分还是很清晰 , 修改demo\_lib.go文件的package为lib

```go
package lib

import "fmt"

func Hello(name string) {
    fmt.Printf("Hello,%s!\n", name)
}
```

#### 代码包的导入路径总会与其所在目录的相对路径一致吗 ?

库源码文件demo\_lib.go所在目录的相对路径是ch1/demo/libs , 而它却声明自己属于lib包 . 在这种情况下 , 该包的导入路径到底是哪个呢 ? 先安装一下这个拆分出来的代码包 :

```
go install ch1/demo/libs
```

构建和安装使用相对路径 . 执行完成后 , 会得到

```
pkg/darwin_amd64/ch1/demo/libs.a
```

其中的darwin\_amd64就是工作区的平台相关目录 . 可以看到这里与源码文件所在目录的相对路径是对应的 .

```go
package main

import (
    "ch1/demo/libs"
    "flag"
)

var myName string

func init() {
    flag.StringVar(&myName, "name", "everyone", "set your name")
}
func main() {
    flag.Parse()
    lib.Hello(myName)
}
```

这里需要注意的是 , 在源码文件中声明所属的代码包与其所在目录的名称不同 . 源码文件所在的目录相对于src目录的相对路径就是它的代码包导入路径 , 而实际使用其程序实体时给定的限定符要与它声明所属的代码包名称对应 . 



