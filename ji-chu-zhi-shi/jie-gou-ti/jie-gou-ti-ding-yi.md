# 结构体的定义

Go语言可以通过自定义的方式形成新的类型 , 结构体就是这些类型中的一种复合类型 , 结构体是由零个或多个任意类型的值聚合成的实体 , 每个值都可以称为结构体的成员 .

结构体成员也可以称为“字段” , 这些字段有以下特性 :

* 字段拥有自己的类型和值 ; 
* 字段名必须唯一 ; 
* 字段的类型也可以是结构体 , 甚至是字段所在结构体的类型 . 

使用关键字 type 可以将各种基本类型定义为自定义类型 , 基本类型包括整型、字符串、布尔等 . 结构体是一种复合的基本类型 , 通过 type 定义为自定义类型后 , 使结构体更便于使用 .

结构体的定义格式 :

```go
type 类型名 struct {
    字段1 字段1类型
    字段2 字段2类型
    …
}
```

对各个部分的说明 :

* 类型名 : 标识自定义结构体的名称 , 在同一个包内不能重复 .
* `struct{}` : 表示结构体类型 , `type 类型名 struct{}`可以理解为将 struct{} 结构体定义为类型名的类型 . 
* 字段1、字段2…… : 表示结构体字段名 , 结构体中的字段名必须唯一 . 
* 字段1类型、字段2类型…… : 表示结构体各个字段的类型 . 

定义示例 :

```go
type Point struct {
    X int
    Y int
}
```

```go
type Color struct {
    R, G, B byte
}
```

结构体的定义只是一种内存布局的描述 , 只有当结构体实例化时 , 才会真正地分配内存 .

```go
package main

import "fmt"

type Books struct {
	title   string
	author  string
	subject string
	bookId  int
}

func main() {
	// 创建一个新的结构体
	fmt.Println(Books{"Go语言", "Google", "Go语言教程", 123456})

	// 使用key=>value格式创建
	fmt.Println(Books{title:"Go语言", author: "Google", subject: "Go语言教程", bookId: 123456})

	// 忽略的字段为0或空
	fmt.Println(Books{title: "Go语言", author: "Google"})
}
```



