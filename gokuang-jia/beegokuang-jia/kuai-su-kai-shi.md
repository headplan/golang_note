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
* 监听服务端口 : 最后一步也就是我们看到的访问 8080 看到的网页端口，内部其实调用了
  `ListenAndServe`, 充分利用了 goroutine 的优势 . 

#### Controller运行机制

**控制器源码**

```go
package controllers

import (
        "github.com/astaxie/beego"
)

type MainController struct {
        beego.Controller
}

func (this *MainController) Get() {
        this.Data["Website"] = "beego.me"
        this.Data["Email"] = "astaxie@gmail.com"
        this.TplName = "index.tpl"
}
```

首先声明控制器MainController , 控制器中内嵌了beego.Controller , 也是Go的组合方式 , 也就是说 , MainController自动拥有了所有beego.Controller的方法 . 其中包括 :

```
Init、Prepare、Post、Get、Delete、Head...
```

可以通过重写的方式来实现这些方法 , 上面的代码就是重写了Get方法 .

beego是一个RESTful的框架 , 所以请求默认是执行对应req.Method的方法 . 浏览器的是`GET`请求 , 那么默认就会执行`MainController`下的`Get`方法 , 这样上面写的Get方法就会被执行到 , 处理我们自己写的逻辑 .

例如 , 上面的代码只是简单的赋值 , 最后一步 , this.TplName就是需要渲染的模板 , 这里指定了index.tpl , 如果不设置 , 会自动到Controller/&lt;方法名&gt;.tpl查找 . 例如 , 上面的方法会去找maincontroller/get.tpl\(文件 , 文件夹必须小写\) .

用户设置了模板之后系统会自动的调用`Render`函数\(这个函数是在 beego.Controller 中实现的\) , 所以无需用户自己来调用渲染 .

也可以不使用模板 , 直接用this.Ctx.WriteString输出字符串 . 例如 :

```go
func (this *MainController) Get() {
    this.Ctx.WriteString("hello")
}
```

#### Model逻辑

如果应用足够简单 , 那么Controller可以处理一切的逻辑 , 如果逻辑里面存在着可以复用的东西 , 那么就抽取出来变成一个模块 . 因此Model就是逐步抽象的过程 , 一般会在Model里面处理一些数据读取 , 例如 :

```go
package models

import (
    "loggo/utils"
    "path/filepath"
    "strconv"
    "strings"
)

var (
    NotPV []string = []string{"css", "js", "class", "gif", "jpg", "jpeg", "png", "bmp", "ico", "rss", "xml", "swf"}
)

const big = 0xFFFFFF

func LogPV(urls string) bool {
    ext := filepath.Ext(urls)
    if ext == "" {
        return true
    }
    for _, v := range NotPV {
        if v == strings.ToLower(ext) {
            return false
        }
    }
    return true
}
```

总的来说 , 如果应用足够简单 , 就不需要Model了 , 如果模块开始多了 , 需要复用 , 需要逻辑分离 , 那么Model是必不可扫的 .

#### View编写

在前面的Controller中 , Get里写过this.TplName = "index.tpl" , 设置显示的模板文件 , 默认支持tpl和html的后缀名 , 如果想设置其他后缀可以调用beego.AddTemplateExt接口设置 .

beego采用了Go语言默认的模板引擎 , 所以和Go的模板语法一样 , 详细可参考《Go Web编程》模板使用指南 :

```
https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/07.4.md
```

**示例代码**

```go
<!DOCTYPE html>

<html>
    <head>
        <title>Beego</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    </head>

    <body>
        <header class="hero-unit" style="background-color:#A9F16C">
            <div class="container">
            <div class="row">
              <div class="hero-text">
                <h1>Welcome to Beego!</h1>
                <p class="description">
                    Beego is a simple & powerful Go web framework which is inspired by tornado and sinatra.
                <br />
                    Official website: <a href="http://{{.Website}}">{{.Website}}</a>
                <br />
                    Contact me: {{.Email}}
                </p>
              </div>
            </div>
            </div>
        </header>
    </body>
</html>
```

在控制器中 , 数据是赋值给data\(map类型\) , 然后在模板中就可以直接通过key访问`.Website`和`.Email`了 .

#### 静态文件处理

网页中包含的静态文件 , 图片 , js , css等的输出 , 需要注册URL前缀和映射的目录\(在/main.go文件中beego.Run\(\)之前加入\) :

```go
StaticDir["/static"] = "static"
```

用户可以设置多个静态文件处理目录 , 假如有多个文件下载目录download1 , download2 , 可以如下映射\(在/main.go文件中beego.Run\(\)之前加入\) : 

```go
beego.SetStaticPath("/down1", "download1")
beego.SetStaticPath("/down2", "download2")
```



