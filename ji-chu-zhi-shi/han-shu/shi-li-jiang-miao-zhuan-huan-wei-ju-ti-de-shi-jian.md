# 示例 - 将秒转换为具体的时间

使用一个数值表示时间中的“秒”值 , 然后使用 resolveTime\(\) 函数将传入的秒数转换为天、小时和分钟等时间单位 . 

```go
package main

import "fmt"

const (
	// 定义每分钟的秒数
	SecondsPerMinute = 60
	// 定义每小时的秒数
	SecondsPreHour = SecondsPerMinute * 60
	// 定义每天的秒数
	SecondsPreDay = SecondsPreHour * 24
)

// 将传入的"秒"解析为三种时间单位
func resolveTime(seconds int) (day, hour, minute int) {
	day = seconds / SecondsPreDay
	hour = seconds / SecondsPreHour
	minute = seconds / SecondsPerMinute

	return
}

func main() {
	// 将返回值作为打印参数
	fmt.Println(resolveTime(1000))

	// 值获取小时和分钟
	_, hour, minute := resolveTime(18000)
	fmt.Println(hour, minute)

	// 只获取天
	day, _, _ := resolveTime(90000)
	fmt.Println(day)
}
```

  




