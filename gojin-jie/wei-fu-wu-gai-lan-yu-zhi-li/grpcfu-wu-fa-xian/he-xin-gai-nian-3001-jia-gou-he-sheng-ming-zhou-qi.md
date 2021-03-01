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

单项 RPC , 即客户端发送一个请求给服务端 , 从服务端获取一个应答 , 就像一次普通的函数调用 . 

```go
rpc SayHello(HelloRequest) returns (HelloResponse){
}
```



