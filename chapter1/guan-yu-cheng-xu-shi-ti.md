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

上面代码中 , 三处代码都声明了相同名称的变量 . 声明重名的变量是无法通过编译的 , 用短变量声明对已有变量进行重声明除外 , 但这只是对于同一个代码块而言的 . 上面声明的三处block变量都在不同的代码块中 , 所以会正常打印出 :

```
The block is inner.
The block is function.
```

打印的变量 , 引用原则也是就近原则 , 先检查当前代码块 , 再检查上级代码块 . 引入的包中 , 相同变量名的引用都有前缀, 可以更好的区分 , 但还有一个特殊情况 .

> 如果我们把代码包导入语句写成import . "XXX"的形式 , 那么就会让这个“XXX”包中公开的程序实体 , 被当前源码文件中的代码 , 视为当前代码包中的程序实体 . 比如 , 如果有代码包导入语句import . fmt , 那么在当前源码文件中引用fmt.Printf函数的时候直接用Printf就可以了 . 在这个特殊情况下 , 程序在查找当前源码文件后会先去查用这种方式导入的那些代码包 .

```go
package main

import . "fmt"

var block = "package"

func main() {
    block := "function"
    {
        block := "inner"
        Printf("The block is %s.\n", block)
    }
    Printf("The block is %s.\n", block)
}
```

#### 不同代码块中的重名变量与变量重声明中的变量的区别

* 变量重声明中的变量一定是在同一个代码块内 - 可重名变量指的是在多个代码块之间 .
* 变量重声明是对同一个变量的多次声明 - 可重名变量中涉及的变量肯定是有多个的 .
* 不论对变量重声明多少次 , 其类型必须始终如一 - 可重名变量之间不存在类似的限制 .
* 变量重声明不存在代码嵌套关系 - 可重名变量所在的代码块之间 , 存在直接或间接的嵌套关系 .

#### 类型断言

前面说了可以重名变量 , 如果可重名变量的类型不同 , 就需要关注一下 . 必要时 , 还需要严格地检查它们的类型 .

```go
package main

import "fmt"

var container = []string{"zero", "one", "two"}

func main() {
    container := map[int]string{0: "zero", 1: "one", 2: "two"}
    fmt.Printf("The element is %q.\n", container[1])
}
```

上面的代码可以正常编译 , 但在打印其中元素之前 , 无法正确判断变量container的类型 . 需要使用“类型断言”表达式 .

```go
value, ok := interface{}(container).([]string)
```

* `interface{}(container)`用来把container变量的值转换为空接口值
* `.([]string)`用于判断前者的类型是否为切片类型`[]string`
* 表达式的结果可以被赋给两个变量 , value和ok . 

> 变量ok是布尔\(bool\)类型的 , 它将代表类型判断的结果 , true或false . 如果是true , 那么被判断的值将会被自动转换为\[\]string类型的值 , 并赋给变量value , 否则value将被赋予nil\(即“空”\) .

这里的ok也可以不写 , 只写一个value  , 但是当判断为否时就会引发异常 . 这种异常在 Go 语言中被叫做panic , 运行时恐慌 .

类型断言表达式的语法形式是

```
x.(T)
```

其中的x代表要被判断类型的值 , **必须为接口类型 , 不是接口类型需要转化成接口类型** .

如果container是某个接口类型的 , 那么这个类型断言表达式就可以是

```
container.([]string)
```

在 Go 语言中 , `interface{}`代表空接口 , 任何类型都是它的实现类型 . 这里的具体语法是

```
interface{}(x)
```

> 一对不包裹任何东西的花括号 , 除了可以代表空的代码块之外 , 还可以用于表示不包含任何内容的数据结构或者说数据类型 .
>
> 比如 :
>
> * `struct{}`代表了不包含任何字段和方法的、空的结构体类型
> * `interface{}`则代表了不包含任何方法定义的、空的接口类型
> * `[]string{}`空切片值
> * `map[int]string{}`空的字典值

圆括号中`[]string`是一个类型字面量 . 所谓类型字面量 , 就是用来表示数据类型本身的若干个字符 . 比如 :

* string是表示字符串类型的字面量
* uint8是表示8位无符号整数类型的字面量
* \[\]string用来表示元素类型为string的切片类型
* map\[int\]string用来表示键类型为int、值类型为string的字典类型

还有更复杂的结构体类型字面量、接口类型字面量等等 . 

```go
package main

import "fmt"

var container = []string{"zero", "one", "two"}

func main() {
	// 方式1
	container := map[int]string{0: "zero", 1: "one", 2: "two"}
	_, ok1 := interface{}(container).([]string)
	_, ok2 := interface{}(container).(map[int]string)
	if !(ok1 || ok2) {
		fmt.Printf("Error: unsupported container type: %T\n", container)
		return
	}
	fmt.Printf("The element is %q. (container type: %T)\n", container[1], container)

	// 方式2
	elem, err := getElement(container)
	if err != nil {
		fmt.Printf("Error: %s\n", err)
		return
	}
	fmt.Printf("The element is %q. (container type: %T)\n", elem, container)
}

func getElement(containerI interface{}) (elem string, err error) {
	switch t := containerI.(type) {
	case []string:
		elem = t[1]
	case map[int]string:
		elem = t[1]
	default:
		err = fmt.Errorf("unsupported container type: %T", containerI)
		return
	}
	return
}
```



