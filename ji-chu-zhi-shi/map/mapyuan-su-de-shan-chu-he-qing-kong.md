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

#### 清空 map 中的所有元素

Go语言中并没有为 map 提供任何清空所有元素的函数、方法 , 清空 map 的唯一办法就是重新 make 一个新的 map . 不过 , 不用担心垃圾回收的效率 , Go语言中的并行垃圾回收效率比写一个清空函数要高效的多 . 



