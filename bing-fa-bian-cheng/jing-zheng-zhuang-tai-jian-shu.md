# 竞争状态简述

有并发就有资源竞争 , 如果两个或者多个 goroutine 在没有相互同步的情况下 , 访问某个共享的资源 , 比如同时对该资源进行读写时 , 就会处于相互竞争的状态 , 这就是并发中的资源竞争 . 

并发本身并不复杂 , 但是因为有了资源竞争的问题 , 就使得我们开发出好的并发程序变得复杂起来 , 因为会引起很多莫名其妙的问题 . 

下面的代码中就会出现竞争状态 : 

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

var (
	count int32
	wg sync.WaitGroup
)

func main() {
	wg.Add(2)
	go incCount()
	go incCount()
	wg.Wait()
	fmt.Println(count)
}

func incCount() {
	defer wg.Done()
	for i := 0; i < 2; i++ {
		value := count
		runtime.Gosched()
		value++
		count = value
	}
}
```



