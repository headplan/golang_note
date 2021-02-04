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

切片长度 len 并不等于切片的容量 cap . 除了在切片的尾部追加 , 还可以在切片的开头添加元素 :

```go
var a = []int{1,2,3}
a = append([]int{0}, a...) // 在开头添加1个元素
a = append([]int{-3,-2,-1}, a...) // 在开头添加1个切片
```

在切片开头添加元素一般都会导致内存的重新分配 , 而且会导致已有元素全部被复制 1 次 , 因此 , 从切片的开头添加元素的性能要比从尾部追加元素的性能差很多 .

因为 append 函数返回新切片的特性 , 所以切片也支持链式操作 , 可以将多个 append 操作组合起来 , 实现在切片中间插入元素 :

```go
var a []int
a = append(a[:i], append([]int{x}, a[i:]...)...) // 在第i个位置插入x
a = append(a[:i], append([]int{1,2,3}, a[i:]...)...) // 在第i个位置插入切片
```

#### 切片复制

Go语言的内置函数copy\(\)可以将一个数组切片复制到另一个数组切片中 , 如果加入的两个数组切片不一样大 , 就会按照其中较小的那个数组切片的元素个数进行复制 .

```
copy( destSlice, srcSlice []T) int
```

其中 srcSlice 为数据来源切片 , destSlice 为复制的目标\(也就是将 srcSlice 复制到 destSlice\) , 目标切片必须分配过空间且足够承载复制的元素个数 , 并且来源和目标的类型必须一致 , copy\(\) 函数的返回值表示实际发生复制的元素个数 .

```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}
copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置
```

虽然通过循环复制切片元素更直接 , 不过内置的copy\(\)函数使用起来更加方便 , copy\(\)函数的第一个参数是要复制的目标slice , 第二个参数是源slice , 两个slice可以共享同一个底层数组 , 甚至有重叠也没有问题 . 

---

#### Golang中的三个点 '...' 的用法

'...'是go的一种语法糖 . 前面声明数组时 , 可以使用 . 表示根据数字元素个数初始化限制 . 还表示切片slice可以被打散进行传递 .

```go
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

```go
var a []int
a = append(a[:i], append([]int{x}, a[i:]...)...) // 在第i个位置插入x
a = append(a[:i], append([]int{1,2,3}, a[i:]...)...) // 在第i个位置插入切片
```

还用于函数有多个不定参数的情况 , 可以接受多个不确定数量的参数 :

```go
func test1(args ...string) { //可以接受任意个string参数
    for _, v:= range args{
        fmt.Println(v)
    }
}
```



