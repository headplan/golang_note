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

需要强调的是 , range返回的是每个元素的副本 , 而不是直接返回对该元素的引用 . 

```go
// 创建一个整型切片，并赋值
slice := []int{10, 20, 30, 40}
// 迭代每个元素，并显示值和地址
for index, value := range slice {
    fmt.Printf("Value: %d Value-Addr: %X ElemAddr: %X\n", value, &value, &slice[index])
}
```

输出结果为

```go
Value: 10 Value-Addr: 10500168 ElemAddr: 1052E100
Value: 20 Value-Addr: 10500168 ElemAddr: 1052E104
Value: 30 Value-Addr: 10500168 ElemAddr: 1052E108
Value: 40 Value-Addr: 10500168 ElemAddr: 1052E10C
```

因为迭代返回的变量是一个在迭代过程中根据切片依次赋值的新变量 , 所以 value 的地址总是相同的 , 要想获取每个元素的地址 , 需要使用切片变量和索引值\(例如上面代码中的&slice\[index\]\) . 

如果不需要索引值 , 也可以使用下划线`_`来忽略这个值 : 

```go
// 创建一个整型切片，并赋值
slice := []int{10, 20, 30, 40}
// 迭代每个元素，并显示其值
for _, value := range slice {
    fmt.Printf("Value: %d\n", value)
}
```



