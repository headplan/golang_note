# 字符串

一个字符串是一个不可改变的字节序列 , 字符串可以包含任意的数据 , 但是通常是用来包含可读的文本 , 字符串是 UTF-8 字符的一个序列\(当字符为 ASCII 码表上的字符时则占用 1 个字节 , 其它字符根据需要占用 2-4 个字节\) .

UTF-8是一种被广泛使用的编码格式 , 是文本文件的标准编码 , 其中包括XML和JSON在内也都使用该编码 . 由于该编码对占用字节长度的不定性 , 在Go语言中字符串也可能根据需要占用 1 至 4 个字节 , 这与其它编程语言如[C++](http://c.biancheng.net/cplus/)、[Java](http://c.biancheng.net/java/)或者[Python](http://c.biancheng.net/python/)不同\(Java始终使用 2 个字节\) . Go语言这样做不仅减少了内存和硬盘空间占用 , 同时也不用像其它语言那样需要对使用UTF-8字符集的文本进行编码和解码 .

#### 与其他主要编程语言的差异

* string是数据类型 , 不是引用或指针类型
* string是只读的byte slice , len函数返回的是所包含的byte数\(不是字符数\)
* string的byte数组可以存放任何数据 , 任何数据的意思是不仅仅可以存储可见字符 , 例如汉字 , 字母 , 不可见的二进制数据也可以通过string存储 . 

Unicode UTF8

* Unicode是一种字符集\(code point\)
* UTF8是unicode的存储实现\(转换为字节序列的规则\)

> 阮一峰的总结
>
> [http://www.ruanyifeng.com/blog/2007/10/ascii\_unicode\_and\_utf-8.html](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)

字符串是一种值类型 , 且值不可变 , 即创建某个文本后将无法再次修改这个文本的内容 , 更深入地讲 , 字符串是字节的定长数组 .

#### 定义字符串

可以使用双引号`""`来定义字符串 , 字符串中可以使用转义字符来实现换行、缩进等效果，常用的转义字符包括 :

* `\n` : 换行符
* `\r` : 回车符
* `\t` : tab 键
* `\u` 或 `\U` : Unicode 字符
* `\\`: 反斜杠自身

```go
package main

import (
    "fmt"
)

func main() {
    var str = "Golang\nGo语言"
    fmt.Println(str)
}
```

一般的比较运算符\(`==、!=、<、<=、>=、>`\)是通过在内存中按字节比较来实现字符串比较的 , 因此比较的结果是字符串自然编码的顺序 . 字符串所占的字节长度可以通过函数`len()`来获取 , 例如`len(str)`

字符串的内容\(纯字节\)可以通过标准索引法来获取 , 在方括号`[ ]`内写入索引 , 索引从 0 开始计数 :

* 字符串str的第1个字节 : str\[0\]
* 第 i 个字节 : str\[i - 1\]
* 最后 1 个字节 : str\[len\(str\)-1\]

需要注意的是 , 这种转换方案只对纯 ASCII 码的字符串有效 .

> 注意 : 获取字符串中某个字节的地址属于非法行为 , 例如`&str[i]`

#### 字符串拼接符"+"

两个字符串s1和s2可以通过 s := s1 + s2 拼接在一起 . 将 s2 追加到 s1 尾部并生成一个新的字符串 s . 

```go
str := "Beginning of the string " +
"second part of the string"
```

> 提示 : 因为编译器会在行尾自动补全分号 , 所以拼接字符串用的加号“+”必须放在第一行末尾 .

也可以使用“+=”来对字符串进行拼接 : 

```go
s := "hel" + "lo,"
s += "world!"
fmt.Println(s) //输出 “hello, world!”
```

#### 新的数据类型

rune , 取出字符串中的Unicode . 将字符串转换为rune的切片 .

```go
func TestStrings(t *testing.T) {
    var s string
    t.Log(s) // 初始化默认是空字符""
    s = "Hello"
    t.Log(len(s))
    // s[1] = "3" // string是不可变的byte slice,此行代码会报编译错误

    s = "\xE4\xB8\xA5" // 可以存储任何二进制数据
    // s = "\xE4\xBA\xB5\xFF" // 这是任意一个二进制数据,是不可见的,显示乱码
    t.Log(s)
    t.Log(len(s))

    s = "中"
    t.Log(len(s)) // 返回3,是byte数

    c := []rune(s)
    t.Logf("中 Unicode %x", c[0])
    t.Logf("中 UTF8 %x", s)
}
```

**编码与存储**

| 字符 | 中 |
| :--- | :--- |
| Unicode | 0x4E2D |
| UTF-8 | 0xE4B8AD |
| string/\[\]byte | \[0xE4,0xB8,0xAD\] |

**遍历字符串中的rune**

```go
func TestStringtoRune(t *testing.T) {
    s := "走过南闯过北"
    for _, c := range s {
        t.Logf("%[1]c %[1]x", c)
    }
}
```

遍历string的时候 , 实际自动把string转成了rune再遍历 . 而不是根据len\(string\)的byte数组个数 .

#### 常用字符串函数

* strings包 : [https://golang.org/pkg/strings/](https://golang.org/pkg/strings/)
* strconv包 : [https://golang.org/pkg/strconv/](https://golang.org/pkg/strconv/)

```go
func TestStringFn(t *testing.T) {
    s := "A,B,C"
    parts := strings.Split(s, ",")
    for _, part := range parts {
        t.Log(part)
    }

    t.Log(strings.Join(parts, "-"))
}

func TestConvFn(t *testing.T) {
    s := strconv.Itoa(10)

    t.Log("str" + s)
    if i, err := strconv.Atoi("10"); err == nil {
        t.Log(10 + i)
    }
}
```



