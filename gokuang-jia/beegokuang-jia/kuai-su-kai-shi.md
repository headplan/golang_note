# 快速开始

#### 新建项目

beego的项目基本都是通过bee命令来创建的 . 创建项目

```
bee new webpro
```

创建的项目的目录结构就是典型的MVC框架 , main.go是入口文件 .

**运行项目**

```
bee run
```

默认访问8080端口 , Go已经有网络层 , 所以不需要nginx和apache .

#### 路由设置

Go的执行过程

![](/assets/gozhixingguocheng.png)

先来看看入口文件main.go

```go
package main

import (
    _ "webpro/routers"
    "github.com/astaxie/beego"
)

func main() {
    beego.Run()
}
```

这里引入了一个包 , \_ "webpro/routers" , 这个包只引入执行了里面的init函数 , 看一下这个包 :

```go
package routers

import (
    "webpro/controllers"
    "github.com/astaxie/beego"
)

func init() {
    beego.Router("/", &controllers.MainController{})
}
```

其中的beego.Router映射URL到controller , 第一个参数是URL\(用户请求的地址\) , 这里注册的是`/`不带任何参数的URL , 第二个参数是对应的Controller , 也就是将把请求分发到对应控制器执行相应的逻辑 . 注册路由就可以用类似的方式 .

```go
beego.Router("/user", &controllers.UserController{})
```

现在 , 通过访问/user就能执行UserController的逻辑了 .

**beego.Run执行后内部流程**

* 解析配置文件 : 配置app.conf文件 , 配置开启的端口 , 是否开启session , 应用名称等信息 . 
* 执行用户的hookfunc : 执行用户注册的hookfunc , 可以通过函数`AddAPPStartHook`注册自己的启动函数 . 
* 是否开启session : 开启的话会全局初始化 . 
* 是否编译模板 : 启动时根据配置把views目录下的所有模板进行预编译 , 存储在map里 , 提高模板运行效率 . 
* 是否开启文档功能 : 根据 EnableDocs 配置判断是否开启内置的文档路由功能 . 
* 是否启动管理模块 : 启动后会在8088端口做一个内部监听 , 可以通过这个端口查询QPS , CPU , 内存 , GC , goroutine , thread等统计信息 . 
* 监听服务端口

#### Controller运行机制

#### Model逻辑

#### View编写

#### 静态文件处理



