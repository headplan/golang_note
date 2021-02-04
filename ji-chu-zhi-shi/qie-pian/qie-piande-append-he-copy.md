# 切片的append和copy

Go语言的内建函数 append\(\) 可以为切片动态添加元素 :

```go
var a []int
a = append(a, 1) // 追加1个元素
a = append(a, 1, 2, 3) // 追加多个元素, 手写解包方式
a = append(a, []int{1,2,3}...) // 追加一个切片, 切片需要解包
```

不过需要注意的是 , 在使用 append\(\) 函数为切片动态添加元素时 , 如果空间不足以容纳足够多的元素 , 切片就会进行“扩容” , 此时新切片的长度会发生改变 . 扩容的方式前面的共享存储结构中解释的很详细 . 

切片在扩容时，容量的扩展规律是按容量的 2 倍数进行扩充 : 

```go
var numbers []int
for i := 0; i < 10; i++ {
    numbers = append(numbers, i)
    fmt.Printf("len: %d  cap: %d pointer: %p\n", len(numbers), cap(numbers), numbers)
}
```

输出结果为

```go
len: 1  cap: 1 pointer: 0xc0420080e8
len: 2  cap: 2 pointer: 0xc042008150
len: 3  cap: 4 pointer: 0xc04200e320
len: 4  cap: 4 pointer: 0xc04200e320
len: 5  cap: 8 pointer: 0xc04200c200
len: 6  cap: 8 pointer: 0xc04200c200
len: 7  cap: 8 pointer: 0xc04200c200
len: 8  cap: 8 pointer: 0xc04200c200
len: 9  cap: 16 pointer: 0xc042074000
len: 10  cap: 16 pointer: 0xc042074000
```



