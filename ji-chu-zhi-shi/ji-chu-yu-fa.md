# 基础语法

#### Go标记

Go程序可以由多个标记组成 , 可以是关键字 , 标识符 , 常量 , 字符串 , 符号 . 如前面写的程序就是由6个标记组成的 :

```
fmt.Println("hello,world")
```

```
1.fmt
2..
3.Println
4.(
5."hello,world"
6.)
```

#### 行分隔符

在Go程序中 , 一行代表一个语句结束 . 不需要像C家族语言一样以`;`号结尾 . 如果非要将多行语句写在同一行 , 可以使用`;`号分隔 , 但实际开发中不建议使用 .

#### 注释

前面说过了 , `//` 和 `/*...*/`

#### 标识符

标识符用来命名变量、类型等程序实体 . 一个标识符实际上就是一个或是多个字母\(A~Z和a~z\)数字\(0~9\)、下划线\_组成的序列 , 但是第一个字符必须是字母或下划线而不能是数字 .

#### 关键字

下面列举了 Go 代码中会使用到的 25 个关键字或保留字 :

| break | default | func | interface | select |
| :--- | :--- | :--- | :--- | :--- |
| case | defer | go | map | struct |
| chan | else | goto | package | switch |
| const | fallthrough | if | range | type |
| continue | for | import | return | var |

Go 语言的36个预定义标识符 :

| append | bool | byte | cap | close | complex | complex64 | complex128 | uint16 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| copy | false | float32 | float64 | imag | int | int8 | int16 | uint32 |
| int32 | int64 | iota | len | make | new | nil | panic | uint64 |
| print | println | real | recover | string | true | uint | uint8 | uintptr |

* 程序一般由关键字、常量、变量、运算符、类型和函数组成
* 程序中可能会使用到这些分隔符 , 括号 \(\) , 中括号 \[\] 和大括号 {}
* 程序中可能会使用到这些标点符号 , `.`、`,`、`;`、`:` 和 `…`

#### Go语言的空格

声明变量必须用空格隔开

```
var age int;
```

其他地方可以使用空格让程序更易阅读

```
fruit = apples + oranges;
```



