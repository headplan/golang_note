# 从切片中删除元素

Go语言并没有对删除切片元素提供专用的语法或者接口 , 需要使用切片本身的特性来删除元素 , 根据要删除元素的位置有三种情况 , 分别是从开头位置删除、从中间位置删除和从尾部删除 , 其中删除切片尾部的元素速度最快 .

#### 从开头位置删除

删除开头的元素可以直接移动数据指针 :

```
a = []int{1, 2, 3}
a = a[1:] // 删除开头1个元素
a = a[N:] // 删除开头N个元素
```

也可以不移动数据指针 , 但是将后面的数据向开头移动 , 可以用 append 原地完成\(所谓原地完成是指在原有的切片数据对应的内存区间内完成 , 不会导致内存空间结构的变化\) :

```
a = []int{1, 2, 3}
a = append(a[:0], a[1:]...) // 删除开头1个元素
a = append(a[:0], a[N:]...) // 删除开头N个元素
```

还可以用 copy\(\) 函数来删除开头的元素 :

```
a = []int{1, 2, 3}
a = a[:copy(a, a[1:])] // 删除开头1个元素
a = a[:copy(a, a[N:])] // 删除开头N个元素
```

#### 从中间位置删除

对于删除中间的元素 , 需要对剩余的元素进行一次整体挪动 , 同样可以用 append 或 copy 原地完成 :

```go
a = []int{1, 2, 3, ...}
a = append(a[:i], a[i+1:]...) // 删除中间1个元素
a = append(a[:i], a[i+N:]...) // 删除中间N个元素
a = a[:i+copy(a[i:], a[i+1:])] // 删除中间1个元素
a = a[:i+copy(a[i:], a[i+N:])] // 删除中间N个元素
```

#### 从尾部删除

```go
a = []int{1, 2, 3}
a = a[:len(a)-1] // 删除尾部1个元素
a = a[:len(a)-N] // 删除尾部N个元素
```

删除开头的元素和删除尾部的元素都可以认为是删除中间元素操作的特殊情况 , 下面来看一个示例 .

```go
package main

import "fmt"

func main() {
    seq := []string{"a", "b", "c", "d", "e"}
    // 指定删除位置
    index := 2
    // 查看删除位置之前的元素和之后的元素
    fmt.Println(seq[:index], seq[index+1:])
    // 将删除点前后的元素连接起来
    seq = append(seq[:index], seq[index+1:]...)
    fmt.Println(seq)
}
```

代码的删除过程可以使用下图来描述 .

![](/assets/shanchuqiepian.png)

Go语言中删除切片元素的本质是 , 以被删除元素为分界点 , 将前后两个部分的内存重新连接起来 . 

#### 提示

连续容器的元素删除无论在任何语言中 , 都要将删除点前后的元素移动到新的位置 , 随着元素的增加 , 这个过程将会变得极为耗时 , 因此 , 当业务需要大量、频繁地从一个切片中删除元素时 , 如果对性能要求较高的话 , 就需要考虑更换其他的容器了\(如双链表等能快速从删除点删除元素\) . 
