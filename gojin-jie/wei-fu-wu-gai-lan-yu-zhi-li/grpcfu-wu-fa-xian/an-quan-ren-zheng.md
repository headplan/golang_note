# 安全认证

gRPC 被设计成可以利用插件的形式支持多种授权机制 . 本文档对多种支持的授权机制提供了一个概览 , 并且用例子来论述对应API , 最后就其扩展性作了讨论 . 

### 支持的授权机制

#### SSL/TLS

gRPC 集成 SSL/TLS 并对服务端授权所使用的 SSL/TLS 进行了改良 , 对客户端和服务端交换的所有数据进行了加密 . 对客户端来讲提供了可选的机制提供凭证来获得共同的授权 . 

#### OAuth 2.0

gRPC 提供通用的机制\(后续进行描述\)来对请求和应答附加基于元数据的凭证 . 当通过 gRPC 访问 Google API 时 , 会为一定的授权流程提供额外的获取访问令牌的支持 , 这将通过以下代码例子进行展示 . 

> Google OAuth2 凭证应该仅用于连接 Google 的服务 . 把 Google 对应的 OAuth2 令牌发往非 Google 的服务会导致令牌被窃取用作冒充客户端来访问 Google 的服务 .


