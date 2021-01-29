# 变量作用域

作用域为已声明标识符所表示的常量、类型、变量、函数或包在源代码中的作用范围 .

根据变量定义位置的不同 , 可以分为以下三个类型 :

* 函数内定义的变量称为**局部变量**
* 函数外定义的变量称为**全局变量**
* 函数定义中的变量称为**形式参数**

#### 局部变量

在函数体内声明的变量称之为局部变量 , 它们的作用域只在函数体内 , 参数和返回值变量也是局部变量 .

```go
package main

import "fmt"

func main() {
   /* 声明局部变量 */
   var a, b, c int

   /* 初始化参数 */
   a = 10
   b = 20
   c = a + b

   fmt.Printf ("结果： a = %d, b = %d and c = %d\n", a, b, c)
}
```

#### 全局变量

在函数体外声明的变量称之为全局变量 , 全局变量可以在整个包甚至外部包\(被导出后\)使用 .

```go
package main

import "fmt"

/* 声明全局变量 */
var g int

func main() {

   /* 声明局部变量 */
   var a, b int

   /* 初始化参数 */
   a = 10
   b = 20
   g = a + b

   fmt.Printf("结果： a = %d, b = %d and g = %d\n", a, b, g)
}
```

Go 语言程序中全局变量与局部变量名称可以相同 , 但是函数内的局部变量会被优先考虑 .

```go
package main

import "fmt"

/* 声明全局变量 */
var g int = 20

func main() {
   /* 声明局部变量 */
   var g int = 10

   fmt.Printf ("结果： g = %d\n",  g)
}
```

#### 形式参数

形式参数会作为函数的局部变量来使用 . 在定义函数时函数名后面括号中的变量叫做形式参数\(简称形参\) . 形式参数只在函数调用时才会生效 , 函数调用结束后就会被销毁 , 在函数未被调用时 , 函数的形参并不占用实际的存储单元 , 也没有实际值 . 

```go
package main

import "fmt"

/* 声明全局变量 */
var a int = 20;

func main() {
   /* main 函数中声明局部变量 */
   var a int = 10
   var b int = 20
   var c int = 0

   fmt.Printf("main()函数中 a = %d\n",  a);
   c = sum( a, b);
   fmt.Printf("main()函数中 c = %d\n",  c);
}

/* 函数定义-两数相加 */
func sum(a, b int) int {
   fmt.Printf("sum() 函数中 a = %d\n",  a);
   fmt.Printf("sum() 函数中 b = %d\n",  b);

   return a + b;
}
```



