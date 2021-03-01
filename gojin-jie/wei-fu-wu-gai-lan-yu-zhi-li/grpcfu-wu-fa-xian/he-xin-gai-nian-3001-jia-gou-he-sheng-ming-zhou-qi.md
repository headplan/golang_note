# gRPC概念

> 核心概念、架构和生命周期

### 概述 {#overview}

#### 服务定义 {#service-definition}

正如其他 RPC 系统 , gRPC 基于如下思想 : 定义一个服务 , 指定其可以被远程调用的方法及其参数和返回类型 . gRPC 默认使用protocol buffers作为接口定义语言\(IDL\) , 来描述服务接口和有效载荷消息结构 . 如果有需要的话 , 可以使用其他替代方案 .

```go
service HelloService {
  rpc SayHello (HelloRequest) returns (HelloResponse);
}

message HelloRequest {
  required string greeting = 1;
}

message HelloResponse {
  required string reply = 1;
}
```

gRPC 允许你定义四类服务方法 :

* 单项 RPC , 即客户端发送一个请求给服务端 , 从服务端获取一个应答 , 就像一次普通的函数调用 .

```go
rpc SayHello(HelloRequest) returns (HelloResponse){
}
```

* 服务端流式 RPC , 即客户端发送一个请求给服务端 , 可获取一个数据流用来读取一系列消息 . 客户端从返回的数据流里一直读取直到没有更多消息为止 . 

```go
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse){
}
```

* 客户端流式 RPC , 即客户端用提供的一个数据流写入并发送一系列消息给服务端 . 一旦客户端完成消息写入 , 就等待服务端读取这些消息并返回应答 .

```go
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse) {
}
```

* 双向流式 RPC , 即两边都可以分别通过一个读写数据流来发送一系列消息 . 这两个数据流操作是相互独立的 , 所以客户端和服务端能按其希望的任意顺序读写 , 例如 : 服务端可以在写应答前等待所有的客户端消息 , 或者它可以先读一个消息再写一个消息 , 或者是读写相结合的其他方式 . 每个数据流里消息的顺序会被保持 . 

```go
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse){
}
```

#### 使用 API 接口

gRPC 提供 protocol buffer 编译插件 , 能够从一个服务定义的`.proto`文件生成客户端和服务端代码 . 通常 gRPC 用户可以在服务端实现这些API , 并从客户端调用它们 . 

* 在服务侧 , 服务端实现服务接口 , 运行一个 gRPC 服务器来处理客户端调用 . gRPC 底层架构会解码传入的请求 , 执行服务方法 , 编码服务应答 . 
* 在客户侧 , 客户端有一个存根实现了服务端同样的方法 . 客户端可以在本地存根调用这些方法 , 用合适的 protocol buffer 消息类型封装这些参数 . gRPC 来负责发送请求给服务端并返回服务端 protocol buffer 响应 . 

#### 同步 vs 异步

同步 RPC 调用一直会阻塞直到从服务端获得一个应答 , 这与 RPC 希望的抽象最为接近 . 另一方面网络内部是异步的 , 并且在许多场景下能够在不阻塞当前线程的情况下启动 RPC 是非常有用的 . 

在多数语言里 , gRPC 编程接口同时支持同步和异步的特点 . 你可以从每个语言教程和参考文档里找到更多内容\(很快就会有完整文档\) . 

### RPC 生命周期

#### 单项 RPC

首先我们来了解一下最简单的 RPC 形式 : 客户端发出单个请求 , 获得单个响应 . 

* 一旦客户端通过桩调用一个方法 , 服务端会得到相关通知 , 通知包括客户端的元数据 , 方法名 , 允许的响应期限\(如果可以的话\)
* 服务端既可以在任何响应之前直接发送回初始的元数据 , 也可以等待客户端的请求信息 , 到底哪个先发生 , 取决于具体的应用 . 
* 一旦服务端获得客户端的请求信息 , 就会做所需的任何工作来创建或组装对应的响应 . 如果成功的话 , 这个响应会和包含状态码以及可选的状态信息等状态明细及可选的追踪信息返回给客户端 . 
* 假如状态是 OK 的话 , 客户端会得到应答 , 这将结束客户端的调用 .  



