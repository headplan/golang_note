# 闭包与匿名函数

#### 把函数作为值保存到变量中

在Go语言中 , 函数也是一种类型 , 可以和其他类型一样保存在变量中 , 下面的代码定义了一个函数变量 f , 并将一个函数名为 fire\(\) 的函数赋给函数变量 f , 这样调用函数变量 f 时 , 实际调用的就是 fire\(\) 函数 .

```go
package main
import (
    "fmt"
)
func fire() {
    fmt.Println("fire")
}
func main() {
    var f func()
    f = fire
    f()
}
```

代码输出为

```
fire
```

#### 匿名函数

Go语言支持匿名函数 , 即在需要使用函数时再定义函数 , 匿名函数没有函数名只有函数体 , 函数可以作为一种类型被赋值给函数类型的变量 , 匿名函数也往往以变量方式传递 , 这与C语言的回调函数比较类似 , 不同的是Go语言支持随时在代码里定义匿名函数 .

匿名函数是指不需要定义函数名的一种函数实现方式 , 由一个不带函数名的函数声明和函数体组成 , 下面来具体介绍一下匿名函数的定义及使用 .

```go
func(参数列表)(返回参数列表){
    函数体
}
```

匿名函数的定义就是没有名字的普通函数定义 .

#### 在定义时调用匿名函数

```go
func(data int) {
    fmt.Println("hello", data)
}(100)
```

注意第3行`}`后的`(100)` , 表示对匿名函数进行调用 , 传递参数为100

#### 将匿名函数赋值给变量

```go
// 将匿名函数体保存到f()中
f := func(data int) {
    fmt.Println("hello", data)
}
// 使用f()调用
f(100)
```

匿名函数的用途非常广泛 , 它本身就是一种值 , 可以方便地保存在各种容器中实现回调函数和操作封装 .

#### 匿名函数用作回调函数

下面的代码实现对切片的遍历操作 , 遍历中访问每个元素的操作使用匿名函数来实现 , 用户传入不同的匿名函数体可以实现对元素不同的遍历操作 :

```go
package main
import (
    "fmt"
)
// 遍历切片的每个元素, 通过给定函数进行元素访问
func visit(list []int, f func(int)) {
    for _, v := range list {
        f(v)
    }
}
func main() {
    // 使用匿名函数打印切片内容
    visit([]int{1, 2, 3, 4}, func(v int) {
        fmt.Println(v)
    })
}
```

匿名函数作为回调函数的设计在Go语言的系统包中也比较常见 , 例如 strings 包中就有类似的设计 :

```go
func TrimFunc(s string, f func(rune) bool) string {
    return TrimRightFunc(TrimLeftFunc(s, f), f)
}
```

#### 使用匿名函数实现操作封装

```go
package main
import (
    "flag"
    "fmt"
)
var skillParam = flag.String("skill", "", "skill to perform")
func main() {
    flag.Parse()
    var skill = map[string]func(){
        "fire": func() {
            fmt.Println("chicken fire")
        },
        "run": func() {
            fmt.Println("soldier run")
        },
        "fly": func() {
            fmt.Println("angel fly")
        },
    }
    if f, ok := skill[*skillParam]; ok {
        f()
    } else {
        fmt.Println("skill not found")
    }
}
```

### 闭包\(Closure\)

Go语言中闭包是引用了自由变量的函数 , 被引用的自由变量和函数一同存在 , 即使已经离开了自由变量的环境也不会被释放或者删除 , 在闭包中可以继续使用这个自由变量 , 因此简单的说 :

```
函数 + 引用环境 = 闭包
```

同一个函数与不同引用环境组合 , 可以形成不同的实例 :

![](/assets/bibaoyuanshuyinyongpng)

一个函数类型就像结构体一样 , 可以被实例化 , 函数本身不存储任何信息 , 只有与引用环境结合后形成的闭包才具有“记忆性” , 函数是编译期静态的概念 , 而闭包是运行期动态的概念 .

#### 其它编程语言中的闭包

闭包\(Closure\)在某些编程语言中也被称为 Lambda 表达式 .

闭包对环境中变量的引用过程也可以被称为“捕获” , 在C++ 11 标准中 , 捕获有两种类型 , 分别是引用和复制 , 可以改变引用的原值叫做“引用捕获” , 捕获的过程值被复制到闭包中使用叫做“复制捕获” .

在 Lua 语言中 , 将被捕获的变量起了一个名字叫做 Upvalue , 因为捕获过程总是对闭包上方定义过的自由变量进行引用 .

闭包在各种语言中的实现也是不尽相同的 , 在 Lua 语言中 , 无论闭包还是函数都属于 Prototype 概念 , 被捕获的变量以 Upvalue 的形式引用到闭包中 .

C++ 与C\# 中为闭包创建了一个类 , 而被捕获的变量在编译时放到类中的成员中 , 闭包在访问被捕获的变量时 , 实际上访问的是闭包隐藏类的成员 .

#### 在闭包内部修改引用的变量

闭包对它作用域上的变量可以进行修改 , 修改引用的变量会对变量进行实际修改 , 通过下面的例子来理解 :

```go
// 准备一个字符串
str := "hello world"
// 创建一个匿名函数
foo := func() {

    // 匿名函数中访问str
    str = "hello dude"
}
// 调用匿名函数
foo()
```

#### 示例 : 闭包的记忆效应



