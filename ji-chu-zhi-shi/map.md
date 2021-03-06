# Map集合

Map 是一种无序的键值对的集合 . Map 最重要的一点是通过 key 来快速检索数据 , key 类似于索引 , 指向数据的值 .

Map 是一种集合 , 所以可以像迭代数组和切片那样迭代它 . 不过 , Map 是无序的 , 无法决定它的返回顺序 , 这是因为 Map 是使用 hash 表来实现的 . 这个结构也称为关联数组或字典 , 在其他编程语言中也称为字典\(Python\)、hash和HashTable等 .

#### 定义 Map

map 是引用类型 , 可以使用内建函数 make 也可以使用 map 关键字来定义 Map :

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type
_map := map[string]int{"one":1,"two":2,"three":3}
_map1 := map[string]int
_map1["one"] = 1

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type,10) // 第二个参数Initial Capacity
```

和数组不同 , map 可以根据新增的 key-value 动态的伸缩 , 因此它不存在固定长度或者最大限制 , 但是也可以选择标明 map 的初始容量 capacity .

当 map 增长到容量上限的时候 , 如果再增加新的 key-value , map 的大小会自动加 1 , 所以出于性能的考虑 , 对于大的 map 或者会快速扩张的 map , 即使只是大概知道容量 , 也最好先标明 .

注 : 如果不初始化 map , 那么就会创建一个 nil map . nil map 不能用来存放键值对 .

```go
package test

import "testing"

func TestMap(t *testing.T) {
    // 创建Map集合
    var countryCapitalMap map[string]string
    countryCapitalMap = make(map[string]string)

    // 给Map插入key-value对,各个国家对应的首都
    countryCapitalMap["France"] = "巴黎"
    countryCapitalMap["Italy"] = "罗马"
    countryCapitalMap["Japan"] = "东京"
    countryCapitalMap["India"] = "新德里"

    // 使用键输出地图值
    for country := range countryCapitalMap {
        t.Log(country, "首都是", countryCapitalMap[country]);
    }

    // 查看元素在集合中是否存在
    capital, ok := countryCapitalMap["USA"]
    t.Log(capital, ok)

    if (ok) {
        t.Log(capital)
    } else {
        t.Log("不存在!")
    }
}
```

#### 用切片作为 map 的值

既然一个 key 只能对应一个 value , 而 value 又是一个原始类型 , 那么如果一个 key 要对应多个值时 , 可以使用切片 . 

例如 , 当我们要处理 unix 机器上的所有进程 , 以父进程\(pid 为整形\)作为 key , 所有的子进程\(以所有子进程的 pid 组成的切片\)作为 value . 通过将 value 定义为 \[\]int 类型或者其他类型的切片 , 就可以优雅的解决这个问题 , 示例代码如下所示 : 

```
mp1 := make(map[int][]int)
mp2 := make(map[int]*[]int)
```

#### **Map元素的访问**

与其他主要编程语言的差异

在访问的Key不存在时 , 仍会返回零值 , 不能通过返回nil来判断元素是否存在 .

#### **delete\(\) 函数**

delete\(\)函数用于删除元素集合的元素 , 参数为map和其对应的key .

```go
func TestDeleteMap(t *testing.T) {
    // 创建Map集合
    countryCapitalMap := map[string]string {
        "France": "巴黎",
        "Italy": "罗马",
        "Japan": "东京",
        "India": "新德里",
    }

    for country := range countryCapitalMap {
        t.Log(country, "首都是", countryCapitalMap[country])
    }

    // 删除元素
    delete(countryCapitalMap,"Japan")
    t.Log("法国条目被删除")
    t.Log("删除元素后地图")

    for country := range countryCapitalMap {
        t.Log(country, "首都是", countryCapitalMap[country])
    }
}
```

#### Map与工厂模式

* Map的value可以是一个方法 . 
* 与Go的Dock Type接口方式一起 , 可以方便的实现单一方法对象的工厂模式 . 

```go
func TestMapWithFuncValue(t *testing.T) {
    m := map[int]func(op int) int{}
    m[1] = func(op int) int { return op }
    m[2] = func(op int) int { return op * op }
    m[3] = func(op int) int { return op * op * op }
    t.Log(m[1](2), m[2](2), m[3](2))
}
```

#### 实现Set

Go的内置集合中没有Set实现 , 可以map\[type\]bool

1.元素的唯一性

2.基本操作

* 添加元素
* 判断元素是否存在
* 删除元素
* 元素个数

```go
func TestMapWithFuncValue(t *testing.T) {
    m := map[int]func(op int) int{}
    m[1] = func(op int) int { return op }
    m[2] = func(op int) int { return op * op }
    m[3] = func(op int) int { return op * op * op }
    t.Log(m[1](2), m[2](2), m[3](2))
}

func TestMapForSet(t *testing.T) {
    mySet := map[int]bool{}
    mySet[1] = true

    n := 1
    if mySet[n] {
        t.Logf("%d is existing", n)
    } else {
        t.Logf("%d is not existing", n)
    }
    mySet[3] = true
    t.Log(len(mySet));
    delete(mySet, 1)

    n = 1
    if mySet[n] {
        t.Logf("%d is existing", n)
    } else {
        t.Logf("%d is not existing", n)
    }
}
```



