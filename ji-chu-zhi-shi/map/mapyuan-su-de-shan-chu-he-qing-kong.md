# map元素的删除和清空

Go语言提供了一个内置函数`delete()` , 用于删除容器内的元素 .

#### 使用 delete\(\) 函数从 map 中删除键值对

使用delete\(\)内建函数从map中删除一组键值对 , delete\(\)函数的格式如下 :

```
delete(map, 键)
```

其中 map 为要删除的 map 实例 , 键为要删除的 map 中键值对的键 .

```go
scene := make(map[string]int)
// 准备map数据
scene["route"] = 66
scene["brazil"] = 4
scene["china"] = 960
delete(scene, "brazil")
for k, v := range scene {
    fmt.Println(k, v)
}
```

例子中使用 delete\(\) 函数将 brazil 从 scene 这个 map 中删除了 . 

