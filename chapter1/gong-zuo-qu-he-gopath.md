# 工作区和GOPATH

安装Go需要配置的三个环境变量 :

GOROOT : Go语言安装根目录的路径 , 也就是Go语言的安装路径 .

GOPATH : 若干工作区目录的路径 , 是我们自己定义的工作空间 .

GOBIN : GO程序生成的可执行文件的路径 . \(executable file\)

#### 重点关注GOPATH

GOPATH可以简单的理解成Go语言的工作目录 , 它的值是一个目录的路径 , 也可以是多个目录路径 , 每个目录都代表Go语言的一个工作区\(workspace\) .

利用这些工作区放置Go语言的源码文件\(source file\) , 已经安装后的归档文件\(archive file , 也就是以.a为扩展名的文件\)和可执行文件\(executable file\) . 



