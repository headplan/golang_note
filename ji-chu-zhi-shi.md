# 基础知识

#### 语言环境安装

[https://golang.org/dl/](https://golang.org/dl/)

或者

[https://golang.google.cn/dl/](https://golang.google.cn/dl/)

下载对应版本 . MacOS安装直接运行pkg包下一步即可 .

Linux二进制包安装 :

```
wget -c https://dl.google.com/go/go1.11.2.src.tar.gz
tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
```

**Hello World**

```
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```



