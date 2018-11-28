# 常量

常量是一个简单值的标识符 , 在程序运行时不会被修改的量 . 常量中的数据类型只可以布尔型、数字型\(整数型、浮点型和复数\)和字符串型 .

定义格式 :

```
const identifier [type] = value
```

可以省略类型说明符 \[type\] , 编译器可以根据变量的值来推断其类型 .

```
显式类型定义 const b string = "abc"
隐式类型定义 const b = "abc"
```

多个相同类型的声明可以简写为 :

```
const c_name1, c_name2 = value1, value2
```

常量还可以用作枚举 :

```
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

例如上面的例子中 , 代表未 , 女性 , 男性 .

常量可以用len\(\) , cap\(\) , unsafe.Sizeof\(\)函数计算表达式的值 . 常量表达式中 , 函数必须是内置函数 , 否则编译不过 :

```go
package main

import (
    "fmt"
    "unsafe"
)
const (
    aa = "abc"
    bb = len(aa)
    cc = unsafe.Sizeof(aa)
)

func main() {
    const LENGTH int = 10
    const WIDTH int = 5
    var area int
    const a, b, c = 1, false, "str" // 多重复值

    area = LENGTH * WIDTH
    fmt.Printf("面积为:%d", area)
    println()
    println(a,b,c)
}
```

#### iota

iota特殊常量 , 是一个可以被编译器修改的常量 .

iota在const关键字出现时将被重置为0\(const内部的第一行之前\) , const中每新增一行常量声明将使iota计数一次\(iota可理解为const语句块中的行索引\) .

iota可以被用作枚举值 :

```go
const (
    a = iota
    b = iota
    c = iota
)
```

第一个iota等于0 , 每当iota在新的一行被使用时 , 它的值都会自动加1 , 所以a=0 , b=1 , c=2可以简写 :

```go
const (
    a = iota
    b
    c
)
```

#### iota用法

```go
package main

import "fmt"

func main() {
    const (
        a = iota
        b
        c
        d = "ha"
        e
        f = 100
        g
        h = iota
        i
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```

```go
package main

import "fmt"
const (
    i = 1 << iota
    j = 3 << iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
```

iota 表示从 0 开始自动加 1 , 所以i=1&lt;&lt;0,j=3&lt;&lt;1\(**&lt;&lt;**表示左移的意思\) , 即:i=1, j=6 , 这没问题 , 关键在 k 和 l , 从输出结果看k=3&lt;&lt;2 , l=3&lt;&lt;3 .

简单表述:

* **i=1 : **左移 0 位 , 不变仍为 1 ; 
* **j=3 : **左移 1 位 , 变为二进制 110 , 即 6 ; 
* **k=3 : **左移 2 位 , 变为二进制 1100 , 即 12 ; 
* **l=3 : **左移 3 位 , 变为二进制 11000 , 即 24 . 



