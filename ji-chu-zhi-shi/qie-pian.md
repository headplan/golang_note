# 切片\(Slice\)

Go语言切片是对数组的抽象 .

数组的长度不可改变 , 在特定的场景中这样的集合就不太适用 , Go中提供了更为灵活的内置类型 , 切片 , 也可以理解为动态数组 , 长度是不固定的 , 可以追加元素 , 在追加时可能使切片的容量增大 .

#### 切片内部结构

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

通过内置函数make\(\)初始化切片s,\[\]int 标识为其元素类型为int的切片

---

#### 切片共享存储结构



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

#### 数组 vs. 切片

1. 容量是否可伸缩
2. 是否可以进行比较

```go
func TestSliceComparing(t *testing.T) {
	a := []int{1,2,3,4}
	b := []int{1,2,3,4}
	if a == b {
		t.Log("equal")
	}
}
```



