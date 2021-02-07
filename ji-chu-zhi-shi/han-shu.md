# 函数

函数是基本的代码块 , 用于执行一个任务 . Go语言最少有个main\(\)函数 . 可以通过函数来划分不同功能 , 逻辑上每个函数执行的是指定的任务 . 函数声明告诉了编译器函数的名称 , 返回类型和参数 . Go语言标准库提供了多种可动用的内置的函数 .

#### 函数定义

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

**函数定义解析**

* func : 函数由func开始声明
* function\_name : 函数名称 , 函数名和参数列表一起构成了函数签名
* parameter list : 参数列表 , 参数就像一个占位符 , 当函数被调用时 , 可以将值传递给参数 , 这个值被称为实际参数 . 参数列表指定的是参数类型 , 顺序 , 及参数个数 . 参数是可选的 , 也就是说函数也可以不包含参数 . 
* return\_types : 返回类型 , 函数返回一列值 . return\_types是该列值的数据类型 . 有些功能不需要返回值 , 这种情况下return\_types不是必须的 . 
* 函数体 : 函数定义的代码集合 . 

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

#### 函数返回多个值

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



