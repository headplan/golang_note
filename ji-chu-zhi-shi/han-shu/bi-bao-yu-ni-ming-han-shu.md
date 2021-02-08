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



