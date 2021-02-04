# 共享存储结构

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

**注意**

append\(\)这个函数在 cap 不够用的时候 , 就会重新分配内存以扩大容量 , 如果够用 , 就不会重新分配内存了 .

```go
func main() {
    path := []byte("AAAA/BBBBBBBBB")
    sepIndex := bytes.IndexByte(path,'/')

    dir1 := path[:sepIndex]
    dir2 := path[sepIndex+1:]

    fmt.Println("dir1 =>",string(dir1)) //prints: dir1 => AAAA
    fmt.Println("dir2 =>",string(dir2)) //prints: dir2 => BBBBBBBBB

    dir1 = append(dir1,"suffix"...)

    fmt.Println("dir1 =>",string(dir1)) //prints: dir1 => AAAAsuffix
    fmt.Println("dir2 =>",string(dir2)) //prints: dir2 => uffixBBBB
}
```

在这个例子中 , dir1 和 dir2 共享内存 , 虽然 dir1 有一个 append\(\) 操作 , 但是因为 cap 足够 , 于是数据扩展到了dir2 的空间 . 下面是相关的图示\(注意上图中 dir1 和 dir2 结构体中的 cap 和 len 的变化\)

![](/assets/neicungongxiang.png)

要解决这个问题 , 只需要修改一行代码

```
dir1 := path[:sepIndex:sepIndex]
```

新的代码使用了 Full Slice Expression\(全切片表达式\) , 最后一个参数叫Limited Capacity\(有限容量\) . 于是 , 后续的`append()`操作会导致重新分配内存 .

切片在扩容时 , 容量的扩展规律是按容量的 2 倍数进行扩充 , 例如 1、2、4、8、16……

因为 append 函数返回新切片的特性 , 所以切片也支持链式操作 , 可以将多个 append 操作组合起来 , 实现在切片中间插入元素 :

```go
var a []int
a = append(a[:i], append([]int{x}, a[i:]...)...) // 在第i个位置插入x
a = append(a[:i], append([]int{1,2,3}, a[i:]...)...) // 在第i个位置插入切片
```

每个添加操作中的第二个 append 调用都会创建一个临时切片 , 并将 a\[i:\] 的内容复制到新创建的切片中 , 然后将临时创建的切片再追加到 a\[:i\] 中 .



