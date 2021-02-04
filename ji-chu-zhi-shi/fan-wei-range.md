# 范围Range

Go语言有个特殊的关键字 range , 它可以配合关键字 for 来迭代切片里的每一个元素 :

```go
// 创建一个整型切片,并赋值
slice := []int{10, 20, 30, 40}
// 迭代每一个元素,并显示其值
for index, value := range slice {
    fmt.Printf("Index: %d Value: %d\n", index, value)
}
```

当迭代切片时 , 关键字 range 会返回两个值 , 第一个值是当前迭代到的索引位置 , 第二个值是该位置对应元素值的一份副本 . 

![](/assets/range.png)

