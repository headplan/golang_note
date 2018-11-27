# 变量

变量来源于数学 , 是计算机语言中能储存计算结果或能表示值抽象概念 . 变量可以通过变量名访问 .

Go语言变量名由字母、数字、下划线组成 , 其中首个字母不能为数字 .

#### 变量声明

指定变量类型 , 声明后若不赋值 , 使用默认值 .

```
var v_name v_type
v_name = value
//或
var vname v_type = value
```

根据值自行判定变量类型 .

```
var v_name = value
```

省略var , 使用`:=`声明变量 , 当然 , 这个变量不能是声明过的 , 否则会编译错误 .

```
v_name := value
```

**代码示例**

```go
package main

var a = "我是皮特"
var b string = "Hello Peter"
var c bool

func main() {
    d := 'fuck'
    println(a, b, c, d)
}
```

#### 多变量声明

```
// 类型相同多个变量,非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3
// 和python很像,不需要显示声明类型,自动推断
var vname1, vname2, vname3 = v1, v2, v3
// 出现在:=左侧的变量不应该是已经被声明过的,否则会导致编译错误
vname1, vname2, vname3 := v1, v2, v3

// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```

**代码示例**

```
package main

var x, y int
var (
    a int
    b bool
)

var c, d int = 1, 2
var e, f = 123, "hello"

// 这种不带声明格式的只能在函数体中出现
// g, h := 123, "hello"

func main() {
    g, h := 123, "hello"
    println(x, y, a, b, c, d, e, f, g, h)
}
```

#### 值类型和引用类型

所有像int , float , bool , string这些基本类型都属于值类型 , 使用这些类型的变量直接指向存在内存中的值 :

![](/assets/neicun1.png)

当使用等号将一个变量的值赋值给另一个变量时 , 实际上是在内存中将i的值进行了拷贝 : 

![](/assets/neicun2.png)

