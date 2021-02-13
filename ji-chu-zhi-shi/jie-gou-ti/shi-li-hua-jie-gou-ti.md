# 实例化结构体

结构体的定义只是一种内存布局的描述 , 只有当结构体实例化时 , 才会真正地分配内存 , 因此必须在定义结构体并实例化后才能使用结构体的字段 .

实例化就是根据结构体定义的格式创建一份与格式一致的内存区域 , 结构体实例与实例间的内存是完全独立的 .

Go语言可以通过多种方式实例化结构体 , 根据实际需要可以选用不同的写法 .

#### 基本的实例化形式

结构体本身是一种类型 , 可以像整型、字符串等类型一样 , 以 var 的方式声明结构体即可完成实例化 .

```go
var ins T
```

其中 , T为结构体类型 , ins为结构体的实例 .

用结构体表示的点结构\(Point\)的实例化过程请参见下面的代码 :

```go
type Point struct {
    X int
    Y int
}
var p Point
p.X = 10
p.Y = 20
```

在例子中 , 使用`.`来访问结构体的成员变量 , 如`p.X`和`p.Y`等 , 结构体成员变量的赋值方法与普通变量一致 .

#### 创建指针类型的结构体

Go语言中 , 还可以使用new关键字对类型\(包括结构体、整型、浮点数、字符串等\)进行实例化 , 结构体在实例化后会形成指针类型的结构体 .

使用 new 的格式如下 :

```go
ins := new(T)
```

其中 :

* T 为类型 , 可以是结构体、整型、字符串等 . 
* ins : T 类型被实例化后保存到 ins 变量中 , ins的类型为 \*T , 属于指针 . 

Go语言可以像访问普通结构体一样使用`.`来访问结构体指针的成员 .

```go
type Player struct{
    Name string
    HealthPoint int
    MagicPoint int
}
tank := new(Player)
tank.Name = "Canon"
tank.HealthPoint = 300
```

经过 new 实例化的结构体实例在成员赋值上与基本实例化的写法一致 .

#### Go语言和 C/C++

在 C/C++ 语言中 , 使用 new 实例化类型后 , 访问其成员变量时必须使用`->`操作符 .

在Go语言中 , 访问结构体指针的成员变量时可以继续使用`.` , 这是因为Go语言为了方便开发者访问结构体指针的成员变量 , 使用了语法糖\(Syntactic sugar\)技术 , 将 ins.Name 形式转换为`(*ins).Name` .

#### 取结构体的地址实例化

在Go语言中 , 对结构体进行`&`取地址操作时 , 视为对该类型进行一次 new 的实例化操作 , 取地址格式如下 :

```go
ins := &T{}
```

其中 : 

* T 表示结构体类型 . 
* ins 为结构体的实例 , 类型为 \*T , 是指针类型 . 

```go
type Command struct {
    Name    string    // 指令名称
    Var     *int      // 指令绑定的变量
    Comment string    // 指令的注释
}
var version int = 1
cmd := &Command{}
cmd.Name = "version"
cmd.Var = &version
cmd.Comment = "show version"
```


