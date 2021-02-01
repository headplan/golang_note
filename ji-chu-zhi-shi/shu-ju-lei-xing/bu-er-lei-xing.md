# 布尔类型

一个布尔类型的值只有两种 : true 或 false .

if 和 for 语句的条件部分都是布尔类型的值 , 并且`==`和`<`等比较操作也会产生布尔型的值 .

一元操作符`!`对应逻辑非操作 , 因此`!true`的值为 false , 更复杂一些的写法是`(!true==false) == true` , 实际开发中我们应尽量采用比较简洁的布尔表达式 , 就像用 x 来表示`x==true` . 

```go
var aVar = 10
aVar == 5  // false
aVar == 10 // true
aVar != 5  // true
aVar != 10 // false
```



