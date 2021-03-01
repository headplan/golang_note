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





