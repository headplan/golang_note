# 遍历map

map 的遍历过程使用 for range 循环完成 :

```go
scene := make(map[string]int)
scene["route"] = 66
scene["brazil"] = 4
scene["china"] = 960
for k, v := range scene {
    fmt.Println(k, v)
}
```

遍历对于Go语言的很多对象来说都是差不多的 , 直接使用 for range 语法即可 , 遍历时可以同时获得键和值 , 如只遍历值 , 可以使用下面的形式 :

```
for _, v := range scene {
```

将不需要的键使用`_`改为匿名变量形式 . 只遍历键时 , 使用下面的形式 :

```
for k := range scene {
```

无须将值改为匿名变量形式 , 忽略值即可 .

> 注意 : 遍历输出元素的顺序与填充顺序无关 , 不能期望 map 在遍历时返回某种期望顺序的结果 .

如果需要特定顺序的遍历结果 , 正确的做法是先排序 , 代码如下 :

```go
scene := make(map[string]int)
// 准备map数据
scene["route"] = 66
scene["brazil"] = 4
scene["china"] = 960
// 声明一个切片保存map数据
var sceneList []string
// 将map数据遍历复制到切片中
for k := range scene {
    sceneList = append(sceneList, k)
}
// 对切片进行排序
sort.Strings(sceneList)
// 输出
fmt.Println(sceneList)
```

输出结果为

```
[brazil china route]
```

> sort.Strings 的作用是对传入的字符串切片进行字符串字符的升序排列 .



