# 构造函数

Go语言的类型或结构体没有构造函数的功能 , 但是我们可以使用结构体初始化的过程来模拟实现构造函数 .

其他编程语言构造函数的一些常见功能及特性如下 :

* 每个类可以添加构造函数 , 多个构造函数使用函数重载实现 . 
* 构造函数一般与类名同名 , 且没有返回值 . 
* 构造函数有一个静态构造函数 , 一般用这个特性来调用父类的构造函数 . 
* 对于C++来说 , 还有默认构造函数、拷贝构造函数等 . 

#### 多种方式创建和初始化结构体

> 模拟构造函数重载

如果使用结构体描述猫的特性 , 那么根据猫的颜色和名字可以有不同种类的猫 , 那么不同的颜色和名字就是结构体的字段 , 同时可以使用颜色和名字构造不同种类的猫的实例 , 这个过程可以参考下面的代码 :

```go
type Cat struct {
    Color string
    Name  string
}
func NewCatByName(name string) *Cat {
    return &Cat{
        Name: name,
    }
}
func NewCatByColor(color string) *Cat {
    return &Cat{
        Color: color,
    }
}
```

在这个例子中 , 颜色和名字两个属性的类型都是字符串 , 由于Go语言中没有函数重载 , 为了避免函数名字冲突 , 使用 NewCatByName\(\) 和 NewCatByColor\(\) 两个不同的函数名表示不同的 Cat 构造过程 . 

#### 带有父子关系的结构体的构造和初始化

> 模拟父级构造调用

黑猫是一种猫 , 猫是黑猫的一种泛称 , 同时描述这两种概念时 , 就是派生 , 黑猫派生自猫的种类 , 使用结构体描述猫和黑猫的关系时 , 将猫\(Cat\)的结构体嵌入到黑猫\(BlackCat\)中 , 表示黑猫拥有猫的特性 , 然后再使用两个不同的构造函数分别构造出黑猫和猫两个结构体实例 , 参考下面的代码 : 

```go
type Cat struct {
    Color string
    Name  string
}
type BlackCat struct {
    Cat  // 嵌入Cat, 类似于派生
}
// “构造基类”
func NewCat(name string) *Cat {
    return &Cat{
        Name: name,
    }
}
// “构造子类”
func NewBlackCat(color string) *BlackCat {
    cat := &BlackCat{}
    cat.Color = color
    return cat
}
```



