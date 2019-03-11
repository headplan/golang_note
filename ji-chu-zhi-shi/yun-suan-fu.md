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

