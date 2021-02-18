# 方法和接收器

#### 方法的声明

声明函数时 , 在其名字之前放上一个变量 , 即是一个方法 . 这个附加的参数会将该函数附加到这种类型上 , 即相当于为这种类型定义了一个独占的方法 .

```go
package main

import (
    "fmt"
    "math"
)

type Point struct {
    X, Y float64
}

// traditional function
func Distance(p, q Point) float64 {
    return math.Hypot(q.X-p.X, q.Y-p.Y)
}

// same thing, but as a method of the Point type
func (p Point) Distance(q Point) float64 {
    return math.Hypot(q.X-p.X, q.Y-p.Y)
}

func main() {
    p := Point{1, 2}
    q := Point{4, 6}
    fmt.Println(Distance(p, q)) // "5", function call
    fmt.Println(p.Distance(q))  // "5", method call
}
```

上面的代码里那个附加的参数p , 就是方法的接收器\(receiver\) , 早期的面向对象语言调用一个方法称为向一个对象发送消息 .

在Go语言中 , 并不会像其它语言那样用this或者self作为接收器 . 可以任意的选择接收器的名字 . 建议使用其类型的第一个字母 , 比如这里使用了Point的首字母p .

在方法调用过程中 , 接收器参数一般会在方法名之前出现 . 这和方法声明是一样的 , 都是接收器参数在方法名字之前 . 下面是例子 :

```go
p := Point{1, 2} 
q := Point{4, 6} 
fmt.Println(Distance(p, q)) // "5", function call 
fmt.Println(p.Distance(q)) // "5", method call
```

可以看到 , 上面的两个函数调用都是Distance , 但是却没有发生冲突 . 第一个Distance的调用实际上用的是包级别的函数geometry.Distance , 而第二个则是使用刚刚声明的Point , 调用的是Point类下声明的Point.Distance方法 . 

> 这种p.Distance的表达式叫做**选择器** , 因为他会选择合适的对应p这个对象的Distance方法来执行 . 选择器也会被用来选择一个struct类型的字段 , 比如p.X . 由于方法和字段都是在同一命名空间 , 所以如果我们在这里声明一个X方法的话 , 编译器会报错 , 因为在调用p.X时会有歧义 .



在Go语言中 , 结构体就像是类的一种简化形式 . 在Go语言中有一个概念 , 它和方法有着同样的名字 , 并且大体上意思相同 , Go 方法是作用在接收器\(receiver\)上的一个函数 , 接收器是某种类型的变量 , 因此方法是一种特殊类型的函数 .

接收器类型可以是\(几乎\)任何类型 , 不仅仅是结构体类型 , 任何类型都可以有方法 , 甚至可以是函数类型 , 可以是 int、bool、string 或数组的别名类型 , 但是**接收器不能是一个接口类型** , 因为接口是一个抽象定义 , 而方法却是具体实现 , 如果这样做了就会引发一个编译错误`invalid receiver type…`

**接收器也不能是一个指针类型** , 但是它可以是任何其他允许类型的指针 , 一个类型加上它的方法等价于面向对象中的一个类 , 一个重要的区别是 , 在Go语言中 , 类型的代码和绑定在它上面的方法的代码可以不放置在一起 , 它们可以存在不同的源文件中 , 唯一的要求是它们必须是同一个包的 .

