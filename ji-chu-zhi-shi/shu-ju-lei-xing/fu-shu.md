# 复数

在计算机中 , 复数是由两个浮点数表示的 , 其中一个表示实部\(real\) , 一个表示虚部\(imag\) . Go语言中复数的类型有两种 , 分别是complex128\(64位实数和虚数\)和complex64\(32位实数和虚数\) , 其中complex128为复数的默认类型 .

复数的值由三部分组成 RE + IMi , 其中RE是实数部分 , IM是虚数部分 , RE和IM均为float类型 , 而最后的 i 是虚数单位 .

声明复数的语法格式如下所示 :

```go
var name complex128 = complex(x, y)
```

其中name为复数的变量名 , complex128为复数的类型 , “=”后面的complex为Go语言的内置函数用于为复数赋值 , x、y分别表示构成该复数的两个float64类型的数值 , x为实部 , y为虚部 . 也可以简写为 :

```go
name := complex(x, y)
```

对于一个复数`z := complex(x, y)` , 可以通过Go语言的内置函数`real(z)`来获得该复数的实部 , 也就是x ; 通过`imag(z)`获得该复数的虚部 , 也就是 y .

使用内置的 complex 函数构建复数 , 并使用 real 和 imag 函数返回复数的实部和虚部 :

```go
var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
fmt.Println(x*y)                 // "(-5+10i)"
fmt.Println(real(x*y))           // "-5"
fmt.Println(imag(x*y))           // "10"
```

复数也可以用`==`和`!=`进行相等比较 , 只有两个复数的实部和虚部都相等的时候它们才是相等的 . 

Go语言内置的 math/cmplx 包中提供了很多操作复数的公共方法 , 实际操作中建议大家使用复数默认的complex128类型 , 因为这些内置的包中都使用 complex128 类型作为参数 . 



