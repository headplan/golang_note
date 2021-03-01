# gRPC

> [https://www.grpc.io/docs/](https://www.grpc.io/docs/)
>
> [http://doc.oschina.net/grpc](http://doc.oschina.net/grpc)

gRPC是一个高性能、开源和通用的 RPC 框架 , 面向移动和 HTTP/2 设计 . 目前提供 C、Java 和 Go 语言版本 , 分别是 : grpc, grpc-java, grpc-go . 其中 C 版本支持 C , C++ , Node.js , Python , Ruby , Objective-C , PHP 和 C\# 支持 .

gRPC 基于 HTTP/2 标准设计 , 带来诸如双向流、流控、头部压缩、单 TCP 连接上的多复用请求等特 . 这些特性使得其在移动设备上表现更好 , 更省电和节省空间占用 .

### 开始

欢迎进入 gRPC 的开发文档 , gRPC 一开始由 google 开发 , 是一款语言中立、平台中立、开源的远程过程调用\(RPC\)系统 . 本文档通过快速概述和一个简单的 Hello World 例子来向您介绍 gRPC . 你可以在本站发现更详细的教程和参考文档——文档将会越来越丰富 .

#### 快速开始

为了直观地着手运行 gRPC , 可以从你所选择的语言对应的快速开始入手 , 里面包含创建这个列子的安装指导、快速上手指南等更多内容 .

> [https://github.com/grpc/grpc/tree/master/examples](https://github.com/grpc/grpc/tree/master/examples)

#### gRPC 是什么

在 gRPC 里客户端应用可以像调用本地对象一样直接调用另一台不同的机器上服务端应用的方法 , 使得您能够更容易地创建分布式应用和服务 . 与许多 RPC 系统类似 , gRPC 也是基于以下理念 : 定义一个服务 , 指定其能够被远程调用的方法\(包含参数和返回类型\) . 在服务端实现这个接口 , 并运行一个 gRPC 服务器来处理客户端调用 . 在客户端拥有一个存根能够像服务端一样的方法 .

![](/assets/grpc.png)

gRPC 客户端和服务端可以在多种环境中运行和交互 - 从 google 内部的服务器到你自己的笔记本 , 并且可以用任何 gRPC支持的语言来编写 . 所以 , 你可以很容易地用 Java 创建一个 gRPC 服务端 , 用 Go、Python、Ruby 来创建客户端 . 此外 , Google 最新 API 将有 gRPC 版本的接口 , 使你很容易地将 Google 的功能集成到你的应用里 . 

#### 使用 protocol buffers

gRPC 默认使用 protocol buffers , 这是 Google 开源的一套成熟的结构数据序列化机制\(当然也可以使用其他数据格式如JSON\) . 正如你将在下方例子里所看到的 , 你用 proto files 创建 gRPC 服务 , 用 protocol buffers 消息类型来定义方法参数和返回类型 . 你可以在 Protocol Buffers 文档找到更多关于 Protocol Buffers 的资料 . 

> https://developers.google.com/protocol-buffers/docs/overview



