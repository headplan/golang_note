# 运算符

运算符用于在程序运行时执行数学或逻辑运算 .

Go语言内置的运算符有 :

* 算术运算符
* 关系运算符\(比较运算符\)
* 逻辑运算符
* 位运算符
* 赋值运算符
* 其他运算符

#### 算术运算符

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| + | 相加 | A + B 输出结果 30 |
| - | 相减 | A - B 输出结果 -10 |
| \* | 相乘 | A \* B 输出结果 200 |
| / | 相除 | B / A 输出结果 2 |
| % | 求余 | B % A 输出结果 0 |
| ++ | 自增 | A++ 输出结果 11 |
| -- | 自减 | A-- 输出结果 9 |

> 注意 : Go语言没有前置的`++` , `--` 运算符

```go
package main

import "fmt"

func main() {
    var a int = 21
    var b int = 10
    var c int

    c = a + b
    fmt.Printf("第一行 - c 的值为 %d\n", c)
    c = a - b
    fmt.Printf("第二行 - c 的值为 %d\n", c)
    c = a * b
    fmt.Printf("第三行 - c 的值为 %d\n", c)
    c = a / b
    fmt.Printf("第四行 - c 的值为 %d\n", c)
    c = a % b
    fmt.Printf("第五行 - c 的值为 %d\n", c)
    a ++
    fmt.Printf("第六行 - a 的值为 %d\n", a)
    a = 21   // a重新赋值为21
    a --
    fmt.Printf("第七行 - a 的值为 %d\n", a)
}
```

#### 关系运算符\(比较运算符\)

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| == | 检查两个值是否相等，如果相等返回 True 否则返回 False。 | \(A == B\) 为 False |
| != | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。 | \(A != B\) 为 True |
| &gt; | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。 | \(A &gt; B\) 为 False |
| &lt; | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。 | \(A &lt; B\) 为 True |
| &gt;= | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 | \(A &gt;= B\) 为 False |
| &lt;= | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | \(A &lt;= B\) 为 True |

用`==`比较数组

通常常见的语言中 , 数组都是引用类型 , 不是值类型 . 所以在比较的时候 , 比较的是两个数组的引用 , 而不是比较值是否相同 .

Go语言中 , 两个数组的维数相等 , 是可以比较的 .

* 相同维数且含有相同个数元素的数组才可以比较
* 每个元素都相同的才相等

```go
package test

import "testing"

func TestCompareArray(t *testing.T) {
    a := [...] int{1, 2, 3, 4}
    b := [...] int{1, 3, 4, 5}
    //c := [...] int{1, 2, 3, 4, 5}
    d := [...] int{1, 2, 3, 4}
    t.Log(a == b)
    //t.Log(a == c)
    t.Log(a == d)
}
```

```go
package main

import "fmt"

func main() {
   var a int = 21
   var b int = 10

   if( a == b ) {
      fmt.Printf("第一行 - a 等于 b\n" )
   } else {
      fmt.Printf("第一行 - a 不等于 b\n" )
   }
   if ( a < b ) {
      fmt.Printf("第二行 - a 小于 b\n" )
   } else {
      fmt.Printf("第二行 - a 不小于 b\n" )
   } 

   if ( a > b ) {
      fmt.Printf("第三行 - a 大于 b\n" )
   } else {
      fmt.Printf("第三行 - a 不大于 b\n" )
   }
   /* Lets change value of a and b */
   a = 5
   b = 20
   if ( a <= b ) {
      fmt.Printf("第四行 - a 小于等于 b\n" )
   }
   if ( b >= a ) {
      fmt.Printf("第五行 - b 大于等于 a\n" )
   }
}
```

#### 逻辑运算符

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| && | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 | \(A && B\) 为 False |
| \|\| | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 | \(A \|\| B\) 为 True |
| ! | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 | !\(A && B\) 为 True |

```go
package main

import "fmt"

func main() {
    var a bool = true
    var b bool = false

    if (a && b) {
        fmt.Printf("第一行 - 条件为 true\n" )
    }
    if (a || b) {
        fmt.Printf("第二行 - 条件为 true\n" )
    }
    // 修改ab的值
    a = false
    b = true
    if (a && b) {
        fmt.Printf("第三行 - 条件为 true\n")
    } else {
        fmt.Printf("第三行 - 条件为 false\n")
    }
    if (!(a && b)) {
        fmt.Printf("第四行 - 条件为 true\n")
    }
}
```

#### 位运算符

位运算符对整数在内存中的二进制位进行操作 .

**与其他主要编程语言的差异**

&^ 按位置零

```
1 &^ 0 -- 1
1 &^ 1 -- 0
0 &^ 1 -- 0
0 &^ 0 -- 0

清零运算后面的数为1,则结果均为0
清零运算后面的数为0,则结果不变
```

```go
// 这里标红是因为前面test包下已经定义了这些常量
const (
    _Readable = 1 << iota
    _Writable
    _Executable
)

func TestCompareBin(t *testing.T) {
    a := 7               // 二进制:0111
    a = a &^ _Readable   // 清零可读操作
    a = a &^ _Executable // 清零可执行操作
    t.Log(a&_Readable == _Readable, a&_Writable == _Writable, a&_Executable == _Executable)
}
```

| **&** | **按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。** | **\(A & B\) 结果为 12, 二进制为 0000 1100** |
| :--- | :--- | :--- |
| **\|** | **按位或运算符"\|"是双目运算符。 其功能是参与运算的两数各对应的二进位相或** | **\(A \| B\) 结果为 61, 二进制为 0011 1101** |
| **^** | **按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。** | **\(A ^ B\) 结果为 49, 二进制为 0011 0001** |
| **&lt;&lt;** | **左移运算符"&lt;&lt;"是双目运算符。左移n位就是乘以2的n次方。 其功能把"&lt;&lt;"左边的运算数的各二进位全部左移若干位，由"&lt;&lt;"右边的数指定移动的位数，高位丢弃，低位补0。** | **A &lt;&lt; 2 结果为 240 ，二进制为 1111 0000** |
| **&gt;&gt;** | **右移运算符"&gt;&gt;"是双目运算符。右移n位就是除以2的n次方。 其功能是把"&gt;&gt;"左边的运算数的各二进位全部右移若干位，"&gt;&gt;"右边的数指定移动的位数。** | **A &gt;&gt; 2 结果为 15 ，二进制为 0000 1111** |

下表列出了位运算符 & , \| 和 ^ 的计算

| p | q | p & q | p \| q | p ^ q |
| :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 |

```go
package main

import "fmt"

func main() {
   var a int = 21
   var c int
   c =  a
   fmt.Printf("第 1 行 - =  运算符实例，c 值为 = %d\n", c )
   c +=  a
   fmt.Printf("第 2 行 - += 运算符实例，c 值为 = %d\n", c )
   c -=  a
   fmt.Printf("第 3 行 - -= 运算符实例，c 值为 = %d\n", c )
   c *=  a
   fmt.Printf("第 4 行 - *= 运算符实例，c 值为 = %d\n", c )
   c /=  a
   fmt.Printf("第 5 行 - /= 运算符实例，c 值为 = %d\n", c )
   c  = 200;
   c <<=  2
   fmt.Printf("第 6行  - <<= 运算符实例，c 值为 = %d\n", c )
   c >>=  2
   fmt.Printf("第 7 行 - >>= 运算符实例，c 值为 = %d\n", c )
   c &=  2
   fmt.Printf("第 8 行 - &= 运算符实例，c 值为 = %d\n", c )
   c ^=  2
   fmt.Printf("第 9 行 - ^= 运算符实例，c 值为 = %d\n", c )
   c |=  2
   fmt.Printf("第 10 行 - |= 运算符实例，c 值为 = %d\n", c )
}
```

#### 其他运算符

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| & | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
| \* | 指针变量。 | \*a; 是一个指针变量 |

```go
package main

import "fmt"

func main() {
   var a int = 4
   var b int32
   var c float32
   var ptr *int

   /* 运算符实例 */
   fmt.Printf("第 1 行 - a 变量类型为 = %T\n", a );
   fmt.Printf("第 2 行 - b 变量类型为 = %T\n", b );
   fmt.Printf("第 3 行 - c 变量类型为 = %T\n", c );

   /*  & 和 * 运算符实例 */
   ptr = &a     /* 'ptr' 包含了 'a' 变量的地址 */
   fmt.Printf("a 的值为  %d\n", a);
   fmt.Printf("*ptr 为 %d\n", *ptr);
}
```

#### 运算符优先级

有些运算符拥有较高的优先级 , 二元运算符的运算方向均是从左至右 . 下表列出了所有运算符以及它们的优先级 , 由上至下代表优先级由高到低 : 

| 优先级 | 运算符 |
| :--- | :--- |
| 5 | \* / % &lt;&lt;&gt;&gt;&&^ |
| 4 | + - \| ^ |
| 3 | == != &lt;&lt;= &gt;&gt;= |
| 2 | && |
| 1 | \|\| |



