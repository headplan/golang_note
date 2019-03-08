# 位运算权限判断

常量的基本用法

```go
package test

import "testing"

const (
	Monday = 1 + iota
	Tuesday
	Wednesday
)

const (
	Readable = 1 << iota
	Writable
	Executable
)

func TestConstTry(t *testing.T) {
	t.Log(Monday, Tuesday, Wednesday)
}

func TestConst2Try(t *testing.T) {
	a := 7
	t.Log(a&Readable == Readable, a&Writable == Writable, a&Executable == Executable)
}
```



