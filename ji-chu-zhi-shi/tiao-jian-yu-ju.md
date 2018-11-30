# 条件语句

条件语句需要开发者通过指定一个或多个条件 , 并通过测试条件是否为true来决定是否执行指定语句 , 并在条件为false的情况在执行另外的语句 .

#### if语句

if语句由布尔表达式后紧跟一个或多个语句组成

```go
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
}
```

If 在布尔表达式为 true 时 , 其后紧跟的语句块执行 , 如果为 false 则不执行 .

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10

   /* 使用 if 语句判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" )
   }
   fmt.Printf("a 的值为 : %d\n", a)
}
```

#### if...else 语句

```
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
} else {
  /* 在布尔表达式为 false 时执行 */
}
```

```go
package main

import "fmt"

func main() {
    var a int = 100

    if a < 20 {
        fmt.Printf("a 小于 20\n")
    } else {
        fmt.Printf("a 大于 20\n" )
    }
    fmt.Printf("a 的值为 : %d\n", a)
}
```

#### if 嵌套语句

可以在if或else if语句中嵌入一个或多个if或else if语句

```
if 布尔表达式 1 {
   /* 在布尔表达式1为true时执行 */
   if 布尔表达式 2 {
      /* 在布尔表达式2为true时执行 */
   }
}
```

```go
package main

import "fmt"

func main() {
    // 定义局部变量
    var a int = 100
    var b int = 200

    // 判断条件
    if a == 100 {
        if b == 200 {
            fmt.Printf("a的值为100,b的值为200\n")
        }
    }
    fmt.Printf("a值为:%d\n", a)
    fmt.Printf("b值为:%d\n", b)
}
```

#### switch 语句

switch语句用于基于不同条件执行不同动作 , 每一个case分支都是唯一的 , 从上直下逐一测试 , 直到匹配为止 .

switch语句执行的过程从上至下 , 直到找到匹配项 , 匹配项后面也不需要再加break

```
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

变量var1可以是任何类型 , 而val1和val2则可以是同类型的任意值 . 类型不被局限于常量或整数 , 但必须是相同的类型或者最终结果为相同类型的表达式 .

可以同时测试多个可能符合条件的值 , 使用逗号分割 , 例如case val1, val2, val3

```go
package main

import "fmt"

func main() {
    // 定义全局变量
    var grade string = "B"
    var marks int = 40

    switch marks {
        case 90 :
            grade = "A"
        case 80 :
            grade = "B"
        case 50,60,70 :
            grade = "C"
        default:
            grade = "D"
    }

    switch {
        case grade == "A" :
            fmt.Printf("优秀!\n")
        case grade == "B", grade == "C" :
            fmt.Printf("良好\n")
        case grade == "D" :
            fmt.Printf("及格\n")
        case grade == "F":
            fmt.Printf("不及格\n")
        default:
            fmt.Printf("差\n")
    }

    fmt.Printf("你的等级是%s\n", grade)
}
```

##### Type Switch

判断某个interface变量中实际存储的变量类型

```
switch x.(type){
    case type:
       statement(s);      
    case type:
       statement(s); 
    /* 你可以定义任意个数的case */
    default: /* 可选 */
       statement(s);
}
```

```go
package main

import "fmt"

func main() {
    var x interface{}

    switch i := x.(type) {
        case nil :
            fmt.Printf("x的类型:%T", i)
        case int :
            fmt.Printf("x是int型")
        case float64 :
            fmt.Printf("x是float64型")
        case func(int) float64:
            fmt.Printf("x是func(int)型")
        case bool, string:
            fmt.Printf("x是bool或string型" )
        default:
            fmt.Printf("未知型")
    }
}
```

#### select 语句

select是Go中的一个控制结构 , 可以理解为用于通信的switch语句 . 每个case必须是一个通信操作 , 要么是发送要么是接收 . 

select随机执行一个可运行的case . 如果没有case可运行 , 它将阻塞 , 直到有case可运行 . 一个默认的子句应该总是可运行的 . 

```go
select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s); 
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
```

* 每个case都必须是一个通信
* 所有channel表达式都会被求值
* 所有被发送的表达式都会被求值
* 如果任意某个通信可以进行 , 它就执行 , 其他被忽略
* 如果有多个case都可以运行 , Select会随机公平的选出一个执行 . 其他不会执行
  * 否则 : 
  * 如果有default子句 , 则执行该语句
  * 如果没有default子句 , select将会阻塞 , 直到某个通信可以运行 . Go不会重新对channel或值进行求值

```go
package main

import "fmt"

func main() {
	var c1, c2, c3 chan int
	var i1, i2 int
	select {
		case i1 = <- c1:
			fmt.Printf("received ", i1, " from c1\n")
		case c2 <- i2:
			fmt.Printf("sent ", i2, " to c2\n")
		case i3, ok := (<-c3):  // same as: i3, ok := <-c3
			if ok {
				fmt.Printf("received ", i3, " from c3\n")
			} else {
				fmt.Printf("c3 is closed\n")
			}
		default:
			fmt.Printf("no communication\n")
	}
}
```



