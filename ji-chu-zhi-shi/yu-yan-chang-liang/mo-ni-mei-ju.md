# 模拟枚举

Go语言现阶段没有枚举类型 , 可以使用const+iota 来模拟枚举类型 : 

```go
type Weapon int
const (
     Arrow Weapon = iota    // 开始生成枚举值, 默认为0
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



