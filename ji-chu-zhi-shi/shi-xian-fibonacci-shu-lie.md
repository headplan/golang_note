# 实现Fibonacci数列

Fibonacci数列又称黄金分割数列 , 因数学家列昂纳多·斐波那契\(Leonardoda Fibonacci\)以兔子繁殖为例子而引入 , 故又称为"兔子数列" .

#### 递推公式

斐波那契数列 : 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...

如果设F\(n\)为该数列的第n项\(n∈N\*\) , 那么这句话可以写成如下形式 : F\(n\)=F\(n-1\)+F\(n-2\)

显然这是一个线性递推数列

每一位数都是前两的和

```go
package test

import "testing"

func TestFibList(t *testing.T) {
	a := 1
	b := 1
	t.Log(" ", a)
	for i := 0; i < 5; i++ {
		t.Log(" ", b)
		a, b = b, a
		b = a + b
	}
}


```

并行赋值简化写法

```go
package test

import "testing"

func TestFibList2(t *testing.T) {
	var tmp int
	a := 1
	for i := 0; i < 5; i++ {
		t.Log(" ",a)
		a,tmp = tmp + a,a
	}
}

```



