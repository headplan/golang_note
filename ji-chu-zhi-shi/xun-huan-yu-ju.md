# 循环语句

for循环是一个循环控制结构 , 可以执行指定次数的循环 .

Go语言的for循环有三种形式 :

和C语言的for一样 :

```
for init; condition; post { }
```

和C的while一样 :

```
for condition { }
```

和C的for\(;;\)一样 :

```
for { }
```

* init : 一般为赋值表达式 , 给控制变量赋初值 ; 
* condition : 关系表达式或逻辑表达式 , 循环控制条件 ; 
* post : 一般为赋值表达式 , 给控制变量增量或减量 . 

for语句执行过程如下 :

* 先对表达式1赋初值
* 判别赋值表达式init是否满足给定条件 , 若其值为真 , 满足循环条件 , 则执行循环体内语句 , 然后执行post , 进入第二次循环 , 再判别 condition ; 否则判断 condition 的值为假 , 不满足条件 , 就终止for循环 , 执行循环体外语句 . 

for循环的range格式可以对slice、map、数组、字符串等进行迭代循环 :

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

![](/assets/go-for.png)

```go
package main

import "fmt"

func main () {
    var a int
    var b int = 15

    numbers := [6] int {1, 2, 3, 5}

    for a := 0; a < 10; a++ {
        fmt.Printf("a的值为:%d\n", a)
    }

    for a < b {
        a++
        fmt.Printf("a的值为: %d\n", a)
    }

    for i,x := range numbers {
        fmt.Printf("第 %d 位 x 的值 = %d\n", i, x)
    }
}
```

#### 循环嵌套

```go
for [condition |  ( init; condition; increment ) | Range]
{
   for [condition |  ( init; condition; increment ) | Range]
   {
      statement(s);
   }
   statement(s);
}
```

```go
package main

import "fmt"

func main() {
    var i, j int

    for i = 2; i < 100; i++ {
        for j = 2; j <= (i/j); j++ {
            if (i % j == 0) {
                break;
            }
        }
        if (j > (i/j)) {
            fmt.Printf("%d是素数\n", i);
        }
    }
}
```

#### 循环控制语句

#### break

* 用于循环语句跳出 , 并开始执行循环之后的语句 . 
* break在switch中执行一条case后跳出语句 . 

```go
package main

import "fmt"

func main() {
    var a int = 10

    for a < 20 {
        fmt.Printf("a 的值为 : %d\n", a);
        a++
        if a > 15 {
            break
        }
    }
}
```

#### continue

跳过当前循环执行下一次循环语句 .

```go
package main

import "fmt"

func main() {
    var a int = 10

    for a < 20 {
        if a == 15 {
            a = a + 1
            continue
        }
        fmt.Printf("a 的值为 : %d\n", a)
        a++
    }
}
```

#### goto

goto语句可以无条件地转移到过程中指定的行 . 通常与条件语句配合使用 . 

可用来实现条件转移 , 构成循环 , 跳出循环体等功能 . 

但是 , 在结构化程序设计中一般不主张使用goto语句 , 以免造成程序流程的混乱 , 使理解和调试程序都产生困难 . 

