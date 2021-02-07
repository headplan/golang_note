# 函数

函数是基本的代码块 , 用于执行一个任务 . Go语言最少有个main\(\)函数 . 可以通过函数来划分不同功能 , 逻辑上每个函数执行的是指定的任务 . 函数声明告诉了编译器函数的名称 , 返回类型和参数 . Go语言标准库提供了多种可动用的内置的函数 .

#### 函数定义

```go
func function_name( [parameter list] ) ( [return_types] ) {
   函数体
}
```

**函数定义解析**

* func : 函数由func开始声明
* function\_name : 函数名称 , 函数名和参数列表一起构成了函数签名
* parameter list : 参数列表 , 参数就像一个占位符 , 当函数被调用时 , 可以将值传递给参数 , 这个值被称为实际参数 . 参数列表指定的是参数类型 , 顺序 , 及参数个数 . 参数是可选的 , 也就是说函数也可以不包含参数 . 
* return\_types : 返回类型 , 函数返回一列值 . return\_types是该列值的数据类型 . 有些功能不需要返回值 , 这种情况下return\_types不是必须的 . 
* 函数体 : 函数定义的代码集合 . 

形式参数列表描述了函数的参数名以及参数类型 , 这些参数作为局部变量 , 其值由参数调用者提供 , 返回值列表描述了函数返回值的变量名以及类型 , 如果函数返回一个无名变量或者没有返回值 , 返回值列表的括号是可以省略的 .

```go
// 函数返回两个数的最大值
func max(num1, num2 int) int {
    // 声明局部变量
    var result int
    if num1 > num2 {
        result = num1
    } else {
        result = num2
    }
    return result
}
```

> 当函数执行到代码块最后一行`}`之前或者 return 语句的时候会退出 , 其中 return 语句可以带有零个或多个参数 , 这些参数将作为返回值供调用者使用 , 简单的 return 语句也可以用来结束 for 的死循环 , 或者结束一个协程\(goroutine\) .

如果一个函数在声明时 , 包含返回值列表 , 那么该函数必须以 return 语句结尾 , 除非函数明显无法运行到结尾处 , 例如函数在结尾时调用了 panic 异常或函数中存在无限循环 .

如果一组形参或返回值有相同的类型 , 不必为每个形参都写出参数类型 :

```go
func f(i, j, k int, s, t string) { /* ... */ }
func f(i int, j int, k int, s string, t string) { /* ... */ }
```

空白标识符`_`可以强调某个参数未被使用

```go
func add(x int, y int) int {return x + y}
func sub(x, y int) (z int) { z = x - y; return}
func first(x int, _ int) int { return x }
func zero(int, int) int { return 0 }
fmt.Printf("%T\n", add) // "func(int, int) int"
fmt.Printf("%T\n", sub) // "func(int, int) int"
fmt.Printf("%T\n", first) // "func(int, int) int"
fmt.Printf("%T\n", zero) // "func(int, int) int"
```

在函数中 , 实参通过值传递的方式进行传递 , 因此函数的形参是实参的拷贝 , 对形参进行修改不会影响实参 . 但是 , 如果实参包括引用类型 , 如指针、slice\(切片\)、map、function、channel等类型 , 实参可能会由于函数的间接引用被修改 .

#### 函数返回值

Go语言支持多返回值 , 多返回值能方便地获得函数执行后的多个返回参数 , Go语言经常使用多返回值中的最后一个返回参数返回函数执行中可能发生的错误 , 示例代码如下 :

```go
conn, err := connectToNetwork()
```

##### 其它编程语言中函数的返回值

* C/C++ - 语言中只支持一个返回值 , 在需要返回多个数值时 , 则需要使用结构体返回结果 , 或者在参数中使用指针变量 , 然后在函数内部修改外部传入的变量值 , 实现返回计算结果 , C++ 语言中为了安全性 , 建议在参数返回数据时使用“引用”替代指针 . 
* C\# - 语言也没有多返回值特性 , C\# 语言后期加入的 ref 和 out 关键字能够通过函数的调用参数获得函数体中修改的数据 . 
* lua 语言没有指针 , 但支持多返回值 , 在大块数据使用时方便很多 . 

Go语言既支持安全指针 , 也支持多返回值 , 因此在使用函数进行逻辑编写时更为方便 .

```go
package test

import "testing"

func TestFunc(t *testing.T) {
    a, b := swap("Mahesh", "Kumar")

    t.Log(max(1, 2))
    t.Log(a, b)
}

// 函数返回两个数的最大值
func max(num1, num2 int) int {
    // 声明局部变量
    var result int
    if num1 > num2 {
        result = num1
    } else {
        result = num2
    }
    return result
}

// 函数返回多个值
func swap(x, y string) (string, string) {
    return x, y
}
```

##### 同一种类型返回值

如果返回值是同一种类型 , 则用括号将多个返回值类型括起来 , 用逗号分隔每个返回值的类型 . 

```go
func typedTwoValues() (int, int) {
    return 1, 2
}
func main() {
    a, b := typedTwoValues()
    fmt.Println(a, b)
}
```

#### 函数调用

当创建函数时 , 定义了函数需要做什么 , 通过调用该函数来执行指定任务 .

调用函数 , 向函数传递参数 , 并返回值 , 例如 :

```go
package test

import "testing"

func TestFunc(t *testing.T) {
    t.Log(max(1,2))
}

// 函数返回两个数的最大值
func max(num1, num2 int) int {
    // 声明局部变量
    var result int
    if num1 > num2 {
        result = num1
    } else {
        result = num2
    }
    return result
}
```

#### 函数参数

函数如果使用参数 , 该变量可称为函数的形参 .

形参就像定义在函数体内的局部变量 .

调用函数 , 可以通过两种方式来传递参数 :

**值传递**

值传递是指在调用函数时将实际参数复制一份传递到函数中 , 这样在函数中如果对参数进行修改 , 将不会影响到实际参数 .

**引用传递**

引用传递是指在调用函数时将实际参数的地址传递到函数中 , 那么在函数中对参数所进行的修改 , 将影响到实际参数 .

```go
func TestReferenceValue(t *testing.T) {
    var a int = 100
    var b int = 200
    t.Log(a, b)

    /**
     * 调用swap2()函数
     * &a 指向 a 指针，a 变量的地址
     * &b 指向 b 指针，b 变量的地址
     */
    swap2(&a, &b)
    t.Log(a, b)
}

func swap2(x *int, y *int) {
    var temp int // 保存x地址上的值
    temp = *x    // 将y值赋给x
    *x = *y      // 将y值赋给x
    *y = temp    // 将temp值赋给y
}
```

默认情况下 , Go语言使用的是值传递 , 即在调用过程中不会影响到实际参数 .

#### 函数用法

* 函数作为值 : 函数定义后可作为值来使用

```go
func TestValueFunc(t *testing.T) {
    getSquareRoot := func(x float64) float64 {
        return math.Sqrt(x)
    }

    t.Log(getSquareRoot(9))
}
```

* 闭包 : 闭包是匿名函数 , 可在动态编程中使用
* 方法 : 方法就是一个包含了接受者的函数

```

```



