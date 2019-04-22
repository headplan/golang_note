# 字符串

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



