# 函数参数

函数如果使用参数 , 该变量可称为函数的形参 .

形参就像定义在函数体内的局部变量 .

调用函数 , 可以通过两种方式来传递参数 :

**值传递**

值传递是指在调用函数时将实际参数复制一份传递到函数中 , 这样在函数中如果对参数进行修改 , 将不会影响到实际参数 .

默认情况下 , Go 语言使用的是值传递 , 即在调用过程中不会影响到实际参数 .

```go
package main

import "fmt"

func main() {
	// 定义局部变量
	var a int = 100
	var b int = 200

	fmt.Printf("交换前a的值为:%d\n", a)
	fmt.Printf("交换前a的值为:%d\n", b)

	// 通过调用函数来交换值
	swap(a, b)

	fmt.Printf("交换前a的值为:%d\n", a)
	fmt.Printf("交换前a的值为:%d\n", b)
}

func swap(x, y int) int {
	var temp int

	temp = x
	x = y
	y = temp

	return temp
}
```

输出结果为

```
交换前 a 的值为 : 100
交换前 b 的值为 : 200
交换后 a 的值 : 100
交换后 b 的值 : 200
```

程序中使用的是值传递 , 所以两个值并没有实现交互 , 可以使用引用传递来实现交换效果 . 

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

