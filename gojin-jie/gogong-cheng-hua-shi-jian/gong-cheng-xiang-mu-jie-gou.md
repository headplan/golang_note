# 工程项目结构

#### Standard Go Project Layout\(标准Go项目布局\)

[https://github.com/golang-standards/project-layout/blob/master/README\_zh.md](https://github.com/golang-standards/project-layout/blob/master/README_zh.md)

这是 Go 应用程序项目的基本布局 . 它不是核心 Go 开发团队定义的官方标准 ; 然而 , 它是 Go 生态系统中一组常见的老项目和新项目的布局模式 . 其中一些模式比其他模式更受欢迎 . 它还具有许多小的增强 , 以及对任何足够大的实际应用程序通用的几个支持目录 .

如果你尝试学习 Go , 或者你正在为自己建立一个PoC或一个玩具项目 , 这个项目布局是没啥必要的 . 从一些非常简单的事情开始\(一个`main.go` 文件绰绰有余\) . 随着项目的增长 , 请记住保持代码结构良好非常重要 , 否则你最终会得到一个凌乱的代码 , 这其中就包含大量隐藏的依赖项和全局状态 . 当有更多的人参与这个项目时 , 你将需要更多的结构 . 这时候 , 介绍一种管理包/库的通用方法是很重要的 . 当你有一个开源项目时 , 或者当你知道其他项目从你的项目存储库中导入代码时 , 这时候拥有私有\(又名`internal`\)包和代码就很重要 . 克隆存储库 , 保留你需要的内容 , 删除其他所有的内容!仅仅因为它在那里并不意味着你必须全部使用它 . 这些模式都没有在每个项目中使用 . 甚至 `vendor`模式也不是通用的 .

Go 1.14 Go Modules 终于可以投入生产了 . 除非你有特定的理由不使用它们 , 否则使用 Go Modules . 如果你使用 , 就无需担心 $GOPATH 以及项目放置的位置 . 存储库中的 go.mod 文件基本假定你的项目托管在 Github 上 , 但这不是要求 . 模块路径可以是任何地方 , 尽管第一个模块路径组件的名称中应该有一个点\(当前版本的Go不再强制使用该模块 , 但如果使用稍旧的版本 , 如果没有 mod 文件构建失败的话 , 不要惊讶\) . 如果你想知道更多信息 , 请参阅Issues 37554和32819 .

> [https://github.com/golang/go/wiki/Modules](https://github.com/golang/go/wiki/Modules)
>
> [https://blog.golang.org/using-go-modules](https://blog.golang.org/using-go-modules)
>
> [https://github.com/golang/go/issues/37554](https://github.com/golang/go/issues/37554)
>
> [https://github.com/golang/go/issues/32819](https://github.com/golang/go/issues/32819)

此项目布局是通用的 , 并且不会尝试强加一个特定的 Go 包结构 .

这是社区的努力 . 如果看到新的模式 , 或者认为一个现有的模式需要更新 , 请提一个 issue .

如果需要命名、格式和样式方面的帮助 , 请运行 [`gofmt`](https://golang.org/cmd/gofmt/)和 [`golint`](https://github.com/golang/lint) . 还要确保阅读这些 Go 代码风格的指导方针和建议 :

* [https://talks.golang.org/2014/names.slide](https://talks.golang.org/2014/names.slide)
* [https://golang.org/doc/effective\_go.html\#names](https://golang.org/doc/effective_go.html#names)
* [https://blog.golang.org/package-names](https://blog.golang.org/package-names)
* [https://github.com/golang/go/wiki/CodeReviewComments](https://github.com/golang/go/wiki/CodeReviewComments)
* [Style guideline for Go packages](https://rakyll.org/style-packages)\(rakyll/JBD\)
* [https://medium.com/golang-learn/go-project-layout-e5213cdcfaa2](https://medium.com/golang-learn/go-project-layout-e5213cdcfaa2)

> gofmt - [https://golang.org/cmd/gofmt/](https://golang.org/cmd/gofmt/)
>
> golint - [https://github.com/golang/lint/](https://github.com/golang/lint/)

更多关于包的命名和组织以及其他代码结构的建议 :

* [GopherCon EU 2018: Peter Bourgon - Best Practices for Industrial Programming](https://www.youtube.com/watch?v=PTE4VJIdHPg)
  * [https://www.youtube.com/watch?v=PTE4VJIdHPg](https://www.youtube.com/watch?v=PTE4VJIdHPg)
* [GopherCon Russia 2018: Ashley McNamara + Brian Ketelsen - Go best practices.](https://www.youtube.com/watch?v=MzTcsI6tn-0)
  * [https://www.youtube.com/watch?v=MzTcsI6tn-0](https://www.youtube.com/watch?v=MzTcsI6tn-0)
* [GopherCon 2017: Edward Muller - Go Anti-Patterns](https://www.youtube.com/watch?v=ltqV6pDKZD8)
  * [https://www.youtube.com/watch?v=ltqV6pDKZD8](https://www.youtube.com/watch?v=ltqV6pDKZD8)
* [GopherCon 2018: Kat Zien - How Do You Structure Your Go Apps](https://www.youtube.com/watch?v=oL6JBUk6tj0)
  * [https://www.youtube.com/watch?v=oL6JBUk6tj0](https://www.youtube.com/watch?v=oL6JBUk6tj0)

#### /cmd

本项目的主干 .

每个应用程序的目录名应该与你想要的可执行文件的名称相匹配\(例如 , `/cmd/myapp`\) .

不要在这个目录中放置太多代码 . 如果你认为代码可以导入并在其他项目中使用 , 那么它应该位于 `/pkg`目录中 . 如果代码不是可重用的 , 或者你不希望其他人重用它 , 请将该代码放到 `/internal`目录中 . 你会惊讶于别人会怎么做 , 所以要明确你的意图 .

通常有一个小的`main`函数 , 从`/internal`和 `/pkg`目录导入和调用代码 , 除此之外没有别的东西 .

Examples :

* [https://github.com/heptio/ark/tree/master/cmd](#)\(just a really small`main`function with everything else in packages\)
* [https://github.com/moby/moby/tree/master/cmd](https://github.com/moby/moby/tree/master/cmd)
* [https://github.com/prometheus/prometheus/tree/master/cmd](https://github.com/prometheus/prometheus/tree/master/cmd)
* [https://github.com/influxdata/influxdb/tree/master/cmd](https://github.com/influxdata/influxdb/tree/master/cmd)
* [https://github.com/kubernetes/kubernetes/tree/master/cmd](https://github.com/kubernetes/kubernetes/tree/master/cmd)
* [https://github.com/satellity/satellity/tree/master/cmd/satellity](https://github.com/satellity/satellity/tree/master/cmd/satellity)
* [https://github.com/dapr/dapr/tree/master/cmd](https://github.com/dapr/dapr/tree/master/cmd)

![](/assets/cmd.png)

#### /internal

私有应用程序和库代码 . 这是你不希望其他人在其应用程序或库中导入代码 . 请注意 , 这个布局模式是由 Go 编译器本身执行的 . 有关更多细节，请参阅Go 1.4[`release notes`](https://golang.org/doc/go1.4#internalpackages)  . 注意 , 你并不局限于顶级`internal`目录 . 在项目树的任何级别上都可以有多个内部目录 . 

你可以选择向 internal 包中添加一些额外的结构 , 以分隔共享和非共享的内部代码 . 这不是必需的\(特别是对于较小的项目\) , 但是最好有可视化的线索来显示预期的包的用途 . 你的实际应用程序代码可以放在 `/internal/app`目录下\(例如`/internal/app/myapp`\) , 这些应用程序共享的代码可以放在`/internal/pkg`目录下\(例如`/internal/pkg/myprivlib`\) . 

因为我们习惯把相关的服务 , 比如账号服务 , 内部有 rpc、job、admin 等 , 相关的服务整合一起后 , 需要区分 app . 单一的服务 , 可以去掉 /internal/myapp . 

