# 类型别名

类型别名是 Go 1.9 版本添加的新功能 , 主要用于解决代码升级、迁移中存在的类型兼容性问题 . 在 C/C++语言中 , 代码重构升级可以使用宏快速定义一段新的代码 , Go语言中没有选择加入宏 , 而是解决了重构中最麻烦的类型名变更问题 .

在 Go 1.9 版本之前定义内建类型的代码是这样写的 :

```
type byte uint8
type rune int32
```

而在 Go 1.9 版本之后变为 :

```
type byte = uint8
type rune = int32
```

这个修改就是配合类型别名而进行的修改 .

#### 区分类型别名与类型定义

定义类型别名的写法为 :

```
type TypeAlias = Type
```

类型别名规定：TypeAlias 只是 Type 的别名 , 本质上 TypeAlias 与 Type 是同一个类型 , 就像一个孩子小时候有小名、乳名，上学后用学名 , 英语老师又会给他起英文名 , 但这些名字都指的是他本人 .

类型别名与类型定义表面上看只有一个等号的差异 , 实际还是有区别的 :

```go
package main
import (
    "fmt"
)
// 将NewInt定义为int类型
type NewInt int
// 将int取一个别名叫IntAlias
type IntAlias = int
func main() {
    // 将a声明为NewInt类型
    var a NewInt
    // 查看a的类型名
    fmt.Printf("a type: %T\n", a)
    // 将a2声明为IntAlias类型
    var a2 IntAlias
    // 查看a2的类型名
    fmt.Printf("a2 type: %T\n", a2)
}
```

运行结果为 : 

```
a type: main.NewInt
a2 type: int
```

结果显示 a 的类型是 main.NewInt , 表示 main 包下定义的 NewInt 类型 , a2 类型是 int , IntAlias 类型只会在代码中存在 , 编译完成时 , 不会有 IntAlias 类型 . 

#### 非本地类型不能定义方法



