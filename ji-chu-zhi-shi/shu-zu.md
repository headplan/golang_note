# 数组

Go语言提供了数组类型的数据结构 .

数组是具有**相同唯一类型**的一组**已编号且长度固定**的数据项序列 , 这种类型可以是任意的原始类型 . 整形 , 字符串或自定义类型 .

数组元素可以通过索引\(位置\)来读取\(或者修改\) , 索引从0开始 , 第一个元素索引为0 , 第二个索引为1 , 以此类推 .

#### 声明数组

指定元素类型以及个数 .

```go
var var_name [SIZE] var_type

// 例如
var balance [10] float32
```

#### 初始化数组

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

初始化数组中{}的元素个数不能大于\[\]中的数字 .

如果忽略\[\]中的数字不设置数组大小 , Go语言会根据元素的个数来设置数组的大小 . 例如 , 上面的例子也可以写成 :

```go
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

---

```
var a [3]int // 声明并初始化为默认值零
a[0] = 1

b := [3]int{1,2,3} // 声明同时初始化
c := [2][2]int{{1,2},{3,4}} // 多维数组初始化
```

数组声明初始化以及遍历

```go
package test

import "testing"

func TestArrayInit(t *testing.T) {
    var arr [3]int
    arr1 := [4]int{1, 2, 3, 4}
    arr3 := [...]int{1, 3, 4, 5}
    arr1[1] = 5
    t.Log(arr[1], arr[2])
    t.Log(arr1, arr3)
}

func TestArrayTravel(t *testing.T) {
    arr := [...]int{1, 3, 4, 5}

    for i := 0; i < len(arr); i++ {
        t.Log(arr[i])
    }

    for _, e := range arr {
        t.Log(e)
    }
}
```

#### 数组截取

arr\[开始索引\(包含\) , 结束索引\(不包含\)\]

```go
a := [...]int{1,2,3,4,5}
a[1:2]
a[1:3]
a[1:len(a)]
a[1:]
a[:3]

// 不支持负数
func TestArraySection(t *testing.T) {
    arr := [...]int{1, 2, 3, 4, 5}
    arr_sec := arr[1:3]
    arr_sec2 := arr[1:]
    arr_sec3 := arr[:4]
    arr_sec4 := arr[1:len(arr)]
    arr_sec5 := arr[:]
    t.Log(arr_sec, arr_sec2, arr_sec3, arr_sec4, arr_sec5)
}
```

#### 多维数组



