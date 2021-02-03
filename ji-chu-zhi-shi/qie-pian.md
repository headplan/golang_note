# 切片\(Slice\)

Go语言切片是对数组的抽象 . 是对数组的一个连续片段的引用 , 所以切片是一个引用类型\(因此更类似于 C/C++中的数组类型或Python中的 list 类型\) .

数组的长度不可改变 , 在特定的场景中这样的集合就不太适用 , Go中提供了更为灵活的内置类型 , 切片 , 也可以理解为动态数组 , 长度是不固定的 , 可以追加元素 , 在追加时可能使切片的容量增大 .

Go语言中切片的内部结构包含地址、大小和容量 , 切片一般用于快速地操作一块数据集合 , 如果将数据集合比作切糕的话 , 切片就是你要的“那一块” , 切的过程包含从哪里开始\(切片的起始位置\)及切多大\(切片的大小\) , 容量可以理解为装切片的口袋大小 .

#### 切片内部结构

```go
type slice struct {
    array unsafe.Pointer //指向存放数据的数组指针
    len   int            //长度有多大
    cap   int            //容量有多大
}
```

![](/assets/qiepainneibujiegou.png)

#### 定义切片

可以声明一个未指定大小的数组来定义切片 :

```
var identifier []type
```

切片不需要说明长度 .

使用make\(\)函数也可以创建切片 :

```
var slice1 []type = make([]type, len)

// 也可以简写为

slice1 := make([]type, len)
```

也可以指定容量 , 其中capacity为可选参数 :

```
make([]T, length, capacity)
```

这里len是数组的长度并且也是切片的初始长度 .

```
var s0 []int
s0 = append(s0, 1)

s := []int{}
s1 := []int{1,2,3}
s2 := make([]int,2,4)
// []type, len, cap
其中len个元素会被初始化为默认零值,未初始化元素不可以访问
```

#### 切片初始化

```
s :=[] int {1,2,3}
```

直接初始化切片 , \[\]表示是切片类型 , {1,2,3}初始化值依次是1,2,3 . 其中cap = len = 3

```
s := arr[:]
```

初始化切片s , 是数组arr的引用 .

```
s := arr[startIndex:endIndex]
```

将arr中从下标startIndex到endIndex-1下的元素创建为一个新的切片

```
s := arr[startIndex:]
```

缺省endIndex时将表示一直到arr的最后一个元素

```
s := arr[:endIndex]
```

缺省startIndex时将表示从arr的第一个元素开始

```
s1 := s[startIndex:endIndex]
```

通过切片s初始化切片s1

```
s :=make([]int,len,cap)
```

通过内置函数make\(\)初始化切片s,\[\]int 标识为其元素类型为int的切片 .

#### len\(\)函数和cap\(\)函数

切片是可索引的 , 并且可以由len\(\)方法获取长度 .

切片提供了计算容量的方法cap\(\) , 可以测量切片最长可以达到多少 .

```go
func TestSliceGrowing(t *testing.T) {
    s := []int{}
    for i := 0; i < 10; i++ {
        s = append(s, i)
        t.Log(len(s), cap(s))
    }
}
```

#### 空\(nil\)切片

一个切片在未初始化之前默认为nil , 长度为0 :

```go
func TestSliceNil(t *testing.T) {

    var num []int
    //num := []int{}

    printSlice(num)

    if (num == nil) {
        t.Log("切片是空的")
    }
}

func printSlice(x []int) {
    fmt.Print(len(x),cap(x),x)
}
```

#### 切片截取

可以通过设置下限及上限来设置截取切片_\[lower-bound:upper-bound\]_ , 实例如下 :

```go
func TestSliceArr(t *testing.T) {
    // 原始切片
    num := []int{0,1,2,3,4,5,6,7,8}
    printSlice(num)

    // 子切片从索引1(包含)到索引4(包含)
    t.Log(num[1:4])
    // 默认下限为0
    t.Log(num[:3])
    // 默认上限为len(s)
    t.Log(num[4:])

    num1 := make([]int,0,5)
    printSlice(num1)
    num2 := num[:2]
    printSlice(num2)
    num3 := num[2:5]
    printSlice(num3)
}

func printSlice(x []int) {
    fmt.Print(len(x),cap(x),x)
}
```

#### append\(\)和copy\(\)函数

如果想增加切片的容量 , 必须创建一个新的更大的切片 , 把原来的切片内容都拷贝过来 .

```go
func TestAppendCopySlice(t *testing.T) {
    var num []int
    t.Log(len(num), cap(num), num)

    // 允许追加空切片
    num = append(num, 0)
    t.Log(len(num), cap(num), num)

    // 向切片添加一个元素
    num = append(num, 1)
    t.Log(len(num), cap(num), num)

    // 同时添加多个元素
    num = append(num, 2, 3, 4, 5)
    t.Log(len(num), cap(num), num)

    // 创建切片是之前切片容量的两倍
    num1 := make([]int, len(num), (cap(num))*2)

    // 拷贝num的内容到num1
    copy(num1, num)
    t.Log(len(num1), cap(num1), num1)
}
```

---

#### 切片共享存储结构

熟悉 C/C++ 的同学一定会知道在结构体里用数组指针的问题 , **数据会发生共享** .

![](/assets/qiepiangongxiangcunchujiegou.png)

```go
func TestSliceShareMemory(t *testing.T) {
    year := []string{"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"}
    Q2 := year[3:6]
    t.Log(Q2, len(Q2), cap(Q2))
    summer := year[5:8]
    t.Log(summer, len(summer), cap(summer))

    summer[0] = "Unknow"
    t.Log(Q2)
    t.Log(year)
}
```

##### 数据append操作

```go
a := make([]int, 32)
b := a[1:16]
a = append(a, 1)
a[2] = 42
```

在这段代码中 , 把 a\[1:16\] 的切片赋给 b , 此时 , a 和 b 的内存空间是共享的 . 然后 , 对 a 做了一个 append\(\)的操作 , 这个操作会让 a 重新分配内存 , 这就会导致 a 和 b 不再共享 . 

append\(\)这个函数在 cap 不够用的时候 , 就会重新分配内存以扩大容量 , 如果够用 , 就不会重新分配内存了 . 

#### 数组 vs. 切片

1. 容量是否可伸缩 , 数组的容量不可伸缩 , 切片可以 . 
2. 是否可以进行比较 , 数组可以进行比较 , 切片不可以 , 切片只能跟nil比较 . 

```go
func TestSliceComparing(t *testing.T) {
    a := []int{1,2,3,4}
    b := []int{1,2,3,4}
    if a == nil {
        t.Log("equal")
    } else {
        t.Log(b)
    }
}
```

> 切片不可以比较 , 切片只可以跟nil比较 .



