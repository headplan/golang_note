# 类型转换

在必要以及可行的情况下 , 一个类型的值可以被转换成另一种类型的值 . 由于Go语言不存在隐式类型转换 , 因此所有的类型转换都必须显式的声明 :

```
valueOfTypeB = typeB(valueOfTypeA)
```

类型转换只能在定义正确的情况下转换成功 , 例如从一个取值范围较小的类型转换到一个取值范围较大的类型\(将 int16 转换为 int32\) . 当从一个取值范围较大的类型转换到取值范围较小的类型\(将int32转换为int16或将float32转换为int\) , 会发生精度丢失\(截断\)的情况 .

只有相同底层类型的变量之间可以进行相互转换\(如将 int16 类型转换成 int32 类型\) , 不同底层类型的变量相互转换时会引发编译错误\(如将 bool 类型转换为 int 类型\) :

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    // 输出各数值范围
    fmt.Println("int8 range:", math.MinInt8, math.MaxInt8)
    fmt.Println("int16 range:", math.MinInt16, math.MaxInt16)
    fmt.Println("int32 range:", math.MinInt32, math.MaxInt32)
    fmt.Println("int64 range:", math.MinInt64, math.MaxInt64)

    // 初始化一个32位整型值
    var a int32 = 1047483647
    // 输出变量的十六进制形式和十进制值
    fmt.Printf("int32: 0x%x %d\n", a, a)

    // 将a变量数值转换为十六进制, 发生数值截断
    b := int16(a)
    // 输出变量的十六进制形式和十进制值
    fmt.Printf("int16: 0x%x %d\n", b, b)

    // 将常量保存为float32类型
    var c float32 = math.Pi
    // 转换为int类型, 浮点发生精度丢失
    fmt.Println(int(c))
}
```

结果显示 a 的类型是 main.NewInt , 表示 main 包下定义的 NewInt 类型 , a2 类型是 int , IntAlias 类型只会在代码中存在 , 编译完成时 , 不会有 IntAlias 类型 .

#### 非本地类型不能定义方法

```go
package main
import (
    "time"
)
// 定义time.Duration的别名为MyDuration
type MyDuration = time.Duration
// 为MyDuration添加一个函数
func (m MyDuration) EasySet(a string) {
}
func main() {
}
```

编译上面代码报错

```
cannot define new methods on non-local type time.Duration
```

不能在一个非本地的类型 time.Duration 上定义新方法 , 非本地类型指的就是 time.Duration 不是在 main 包中定义的 , 而是在 time 包中定义的 , 与 main 包不在同一个包中 , 因此不能为不在一个包中的类型定义方法 .

#### 在结构体成员嵌入时使用别名

```go
package main
import (
    "fmt"
    "reflect"
)
// 定义商标结构
type Brand struct {
}
// 为商标结构添加Show()方法
func (t Brand) Show() {
}
// 为Brand定义一个别名FakeBrand
type FakeBrand = Brand
// 定义车辆结构
type Vehicle struct {
    // 嵌入两个结构
    FakeBrand
    Brand
}
func main() {
    // 声明变量a为车辆类型
    var a Vehicle

    // 指定调用FakeBrand的Show
    a.FakeBrand.Show()
    // 取a的类型反射对象
    ta := reflect.TypeOf(a)
    // 遍历a的所有成员
    for i := 0; i < ta.NumField(); i++ {
        // a的成员信息
        f := ta.Field(i)
        // 打印成员的字段名和类型
        fmt.Printf("FieldName: %v, FieldType: %v\n", f.Name, f.Type.
            Name())
    }
}
```

代码输出 : 

```
FieldName: FakeBrand, FieldType: Brand
FieldName: Brand, FieldType: Brand
```

FakeBrand 是 Brand 的一个别名 , 在 Vehicle 中嵌入 FakeBrand 和 Brand 并不意味着嵌入两个 Brand , FakeBrand 的类型会以名字的方式保留在 Vehicle 的成员中 . 

但是在调用 Show\(\) 方法时 , 因为两个类型都有 Show\(\) 方法 , 会发生歧义 , 而编译报错 . 证明 FakeBrand 的本质确实是 Brand 类型 . 

