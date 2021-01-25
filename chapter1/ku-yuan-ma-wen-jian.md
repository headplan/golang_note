# 库源码文件

库源码文件是不能被直接运行的源码文件 , 它仅用于存放程序实体 , 这些程序实体可以被其他代码使用 . 这里的"其他代码"可以与被使用的程序实体在同一个源码文件内 , 也可以在其他源码文件 , 甚至其他代码包中 .

> 在 Go 语言中 , 程序实体是变量、常量、函数、结构体和接口的统称 . 我们总是会先声明\(或者说定义\)程序实体 , 然后再去使用 . 比如 , 先定义了变量name , 然后在main函数中调用fmt.Printf函数的时候用到了它 .
>
> 程序实体的名字被统称为标识符 , 标识符可以是任何 Unicode 编码可以表示的字母字符、数字以及下划线“\_” , 但是其首字母不能是数字 .

#### 怎样把命令源码文件中的代码拆分到其他库源码文件 ?

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
go build demo.go hello_lib.go
./demo --name="test"
```


