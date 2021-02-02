# 模拟枚举

Go语言现阶段没有枚举类型 , 可以使用const+iota 来模拟枚举类型 :

```go
type Weapon int
const (
     Arrow Weapon = iota    // 开始生成枚举值,默认为0
     Shuriken
     SniperRifle
     Rifle
     Blower
)
// 输出所有枚举值
fmt.Println(Arrow, Shuriken, SniperRifle, Rifle, Blower)
// 使用枚举类型并赋初值
var weapon Weapon = Blower
fmt.Println(weapon)
```

iota 不仅可以生成每次增加 1 的枚举值 . 还可以利用 iota 来做一些强大的枚举常量值生成器 .

```go
const (
    FlagNone = 1 << iota
    FlagRed
    FlagGreen
    FlagBlue
)
fmt.Printf("%d %d %d\n", FlagRed, FlagGreen, FlagBlue)
fmt.Printf("%b %b %b\n", FlagRed, FlagGreen, FlagBlue)
```

#### 将枚举值转换为字符串

```go
package main
import "fmt"
// 声明芯片类型
type ChipType int
const (
    None ChipType = iota
    CPU    // 中央处理器
    GPU    // 图形处理器
)
func (c ChipType) String() string {
    switch c {
    case None:
        return "None"
    case CPU:
        return "CPU"
    case GPU:
        return "GPU"
    }
    return "N/A"
}
func main() {
    // 输出CPU的值并以整型格式显示
    fmt.Printf("%s %d", CPU, CPU)
}
```

String\(\) 方法的 ChipType 在使用上和普通的常量没有区别 . 当这个类型需要显示为字符串时 , Go语言会自动寻找 String\(\) 方法并进行调用 . 

