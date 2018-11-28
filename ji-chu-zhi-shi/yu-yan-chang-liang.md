# 常量

常量是一个简单值的标识符 , 在程序运行时不会被修改的量 . 常量中的数据类型只可以布尔型、数字型\(整数型、浮点型和复数\)和字符串型 .

定义格式 :

```
const identifier [type] = value
```

可以省略类型说明符 \[type\] , 编译器可以根据变量的值来推断其类型 .

```
显式类型定义 const b string = "abc"
隐式类型定义 const b = "abc"
```

多个相同类型的声明可以简写为 :

```
const c_name1, c_name2 = value1, value2
```

常量还可以用作枚举 :

```
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

例如上面的例子中 , 代表未 , 女性 , 男性 . 

