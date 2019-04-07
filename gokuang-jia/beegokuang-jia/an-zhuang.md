# 安装

Go安装包的形式 :

```
go get github.com/astaxie/beego
```

常见问题 :

git 没有安装 .

git https 无法获取 , 可以关闭git的https验证 .

```
git config --global http.sslVerify false
```

默认安装在公共的GOPATH中 :

```
/Users/headplan/go
```

#### Beego的升级

Go方式升级\(推荐\)

```
go get -u github.com/astaxie/beego
```

源码覆盖方式升级

```
# 下载源码覆盖后执行
go install  github.com/astaxie/beego
```

## Bee工具

协助快速开发工具 , 用于项目的创建、热编译、开发、测试、和部署 .

#### 安装

```
go get github.com/beego/bee
```

Bee可执行文件默认存放在`$GOPATH/bin`里 , 所以需要把`$GOPATH/bin`添加到环境变量中 , 才可以进行下一步 .

如果本机设置了`GOBIN`目录 , 上面的命令就会安装到`GOBIN`下 , 添加 GOBIN 到环境变量中即可 .

#### 命令详解

```
bee command [arguments]
```

**相关命令**

```
version 展示bee和beego的版本
migrate 运行数据库迁移
api 创建基本的api应用
bale 将非go文件打包为go源文件
fix 修复应用,通过beego新版本使其兼容
dlv 使用Delve启动调试session
dockerize 为Beego应用程序生成Dockerfile
generate 源代码生成器
hprose 基于hprose和beego框架创建RPC应用程序
new 创建应用
pack 压缩zip包应用
rs 运行自定义脚本
run 运行可热编译的应用程序
server 在端口上通过HTTP提供静态内容
```

**new命令**

new 命令是新建一个 Web 项目 , 在命令行下执行 bee new &lt;项目名&gt; 就可以创建一个新的项目 . 但是注意该命令必须在`$GOPATH/src`下执行 . 最后会在`$GOPATH/src`相应目录下生成目录结构的项目 .

```
myproject
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── main.go
├── models
├── routers
│   └── router.go
├── static
│   ├── css
│   ├── img
│   └── js
├── tests
│   └── default_test.go
└── views
    └── index.tpl
```

**api命令**

上面的`new`命令是用来新建 Web 项目 , 这个api命令是用来创建API应用的 .

```
apiproject
├── conf
│   └── app.conf
├── controllers
│   └── object.go
│   └── user.go
├── docs
│   └── doc.go
├── main.go
├── models
│   └── object.go
│   └── user.go
├── routers
│   └── router.go
└── tests
    └── default_test.go
```

目录上看 , 少了static和views目录 , 多了test模块来做单元测试 .

自定义参数自动连接数据库创建相关 model 和 controller :

```
bee api [appname] [-tables=""] [-driver=mysql] [-conn="root:<password>@tcp(127.0.0.1:3306)/test"]
```

其中conn参数为空则创建一个示例项目 . 如果修改Controller下的default.go文件 , 就能看到控制台的输出 . 

**run命令**



