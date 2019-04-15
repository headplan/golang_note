# Map集合

Map 是一种无序的键值对的集合 . Map 最重要的一点是通过 key 来快速检索数据 , key 类似于索引 , 指向数据的值 .

Map 是一种集合 , 所以可以像迭代数组和切片那样迭代它 . 不过 , Map 是无序的 , 无法决定它的返回顺序 , 这是因为 Map 是使用 hash 表来实现的 .

#### 定义 Map

可以使用内建函数 make 也可以使用 map 关键字来定义 Map :

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

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



