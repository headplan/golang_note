# 语言结构

**基础组成部分**

* 包声明
* 引入包
* 函数
* 变量
* 语句&表达式
* 注释

```go
package main

import "fmt"

func main() {
    fmt.Println("hello,world")
}
```

第一行代码package main定义了包名 . 你必须在源文件中非注释的第一行指明这个文件属于哪个包 , 如package main . package main表示一个可独立执行的程序 , 每个Go应用程序都包含一个名为main的包 . 

