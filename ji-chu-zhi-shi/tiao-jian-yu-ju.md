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

#### select 语句



