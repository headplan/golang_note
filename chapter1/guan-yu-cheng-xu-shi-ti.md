# 关于程序实体

Go 语言中的程序实体包括变量、常量、函数、结构体和接口 . Go 语言是静态类型的编程语言 , 所以在声明变量或常量的时候 , 都需要指定它们的类型 , 或者给予足够的信息 , 这样才可以让 Go 语言能够推导出它们的类型 .

> 在 Go 语言中 , 变量的类型可以是其预定义的那些类型 , 也可以是程序自定义的函数、结构体或接口 . 常量的合法类型不多 , 只能是那些 Go 语言预定义的基本类型 . 它的声明方式也更简单一些 .

#### 声明变量的几种方式

最基本的声明方式

```go
var name string
```

还有几种典型的方式

```go
var myName = flag.String("myName", "mySelf", "set my name")
```

> 这里需要注意的是flag.String函数返回的结果值的类型是\*string而不是string . 类型\*string代表的是字符串的指针类型 , 而不是字符串类型 . 因此 , 这里的变量name代表的是一个指向字符串值的指针 .

这里需要通过操作符\*把这个指针指向的字符串值取出来了 , 所以打印的时候需要写成

```go
fmt.Printf("Hello,%v\n", *myName)
```

还有一种声明方式 , 可以省略var关键字

```go
yourName := flag.String("yourName", "yourSelf", "set your name")
```

#### 类型推断和短变量声明

通过 Go 语言自身的类型推断 , 可以省去对该变量的类型的声明 , 也就是上面提到的声明方式 .

> 简单地说 , 类型推断是一种编程语言在编译期自动解释表达式类型的能力 .

可以认为表达式类型就是对表达式进行求值后得到结果的类型 . 它只能用于对变量或常量的初始化 , 就像上述回答中描述的那样 . 对flag.String函数的调用其实就是一个调用表达式 , 而这个表达式的类型是\*string , 即字符串的指针类型 .

> 只能在函数体内部使用**短变量声明**

#### ![](/assets/bianliangshengming.png)Go语言的类型推断可以带来哪些好处 ?

更新一下前面的示例代码

```go
package main

import (
    "flag"
    "fmt"
)

func main() {
    var name string
    var myName = flag.String("myName", "mySelf", "set my name")
    yourName := flag.String("yourName", "yourSelf", "set your name")
    var hisName = getTheFlag()
    flag.StringVar(&name, "name", "everyone", "set name")
    flag.Parse()
    fmt.Printf("Hello,%v\n", name)
    fmt.Printf("Hello,%v\n", *myName)
    fmt.Printf("Hello,%v\n", *yourName)
    fmt.Printf("Hello,%v\n", *hisName)
}

func getTheFlag() *string {
    return flag.String("hisName", "hisSelf", "set his name")
}
```

用getTheFlag函数包裹\(或者说包装\)那个对flag.String函数的调用 , 并把其结果直接作为函数的结果 , 结果的类型是\*string .

这样一来var name = 右边的表达式 , 可以变为针对getTheFlag函数的调用表达式了 . 这实际上是对声明并赋值name变量的那行代码的重构 . 可以随意改变getTheFlag函数的内部实现 , 及其返回结果的类型 , 而不用修改main函数中的任何代码 .

**不显式地指定变量name的类型 , 使得它可以被赋予任何类型的值** . 构建时候 , Go语言会自动地更新变量name的类型 , 在动态的编程语言Python或PHP等中你一定见过这种场景 .

对于Go这种静态类型语言 , 这种类型的确定是在编译期完成的 , 因此不会对程序的运行效率产生任何影响 .

> Go 语言的类型推断可以明显提升程序的灵活性 , 使得代码重构变得更加容易 , 同时又不会给代码的维护带来额外负担\(实际上 , 它恰恰可以避免散弹式的代码修改\) , 更不会损失程序的运行效率 .

#### 变量的重声明

变量重声明是对已经声明过的变量再次声明 .

##### 变量重声明的前提条件

* 由于变量的类型在其初始化时就已经确定了 , 所以对它再次声明时赋予的类型必须与其原本的类型相同 , 否则会产生编译错误 . 
* 变量的重声明只可能发生在某一个代码块中 . 
* 变量的重声明只有在使用短变量声明时才会发生 , 否则也无法通过编译 . 
* 被"声明并赋值"的变量必须是多个 , 并且其中至少有一个是新的变量 . 这时才可以说对其中的旧变量进行了重声明 . 

```go
// 变量重声明
var err error
n, err := io.WriteString(os.Stdout, "Hello, everyone!\n")
if err != nil {
    fmt.Printf("Error: %v\n", err)
}
fmt.Printf("%d byte(s) were written.\n", n)
```

变量重声明其实算是一个语法糖 . 它允许在使用短变量声明时不用理会被赋值的多个变量中是否包含旧变量 . 可以想象 , 如果不这样会多写不少代码 . 

#### 程序实体的作用域

一个代码块可以有若干个子代码块 ; 但对于每个代码块 , 最多只会有一个直接包含它的代码块 , 也就是外层代码块 . 

程序实体的访问权限有三种 : 

* 包级私有
* 模块级私有
* 公开的

更细粒度就是再细化的程序实体的作用域 . 

> 一个程序实体的作用域总是会被限制在某个代码块中 , 而这个作用域最大的用处 , 就是对程序实体的访问权限的控制

也就是高内聚 , 低耦合的程序设计思想 . 

#### 如果一个变量与其外层代码块中的变量重名会出现什么状况

先看代码 : 

```go
package main

import "fmt"

var block = "package"

func main() {
	block := "function"
	{
		block := "inner"
		fmt.Printf("The block is %s.\n", block)
	}
	fmt.Printf("The block is %s.\n", block)
}

```





