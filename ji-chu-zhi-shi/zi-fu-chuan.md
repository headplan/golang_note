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

**编码与存储**

| 字符 | 中 |
| :--- | :--- |
| Unicode | 0x4E2D |
| UTF-8 | 0xE4B8AD |
| string/\[\]byte | \[0xE4,0xB8,0xAD\] |



