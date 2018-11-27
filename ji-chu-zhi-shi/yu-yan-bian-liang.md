# 变量

变量来源于数学 , 是计算机语言中能储存计算结果或能表示值抽象概念 . 变量可以通过变量名访问 .

Go语言变量名由字母、数字、下划线组成 , 其中首个字母不能为数字 .

#### 变量声明

指定变量类型 , 声明后若不赋值 , 使用默认值 .

```
var v_name v_type
v_name = value
//或
var vname v_type = value
```

根据值自行判定变量类型 . 

```
var v_name = value
```

省略var , 使用`:=`声明变量 , 当然 , 这个变量不能是声明过的 , 否则会编译错误 . 

```
v_name := value
```

代码示例

```go
package main

var a = "我是皮特"
var b string = "Hello Peter"
var c bool

func main() {
	println(a, b, c)
}
```



