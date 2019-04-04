# Beego框架

#### 简介

beego 是一个快速开发 Go 应用的 HTTP 框架，他可以用来快速开发 API、Web 及后端服务等各种应用，是一个 RESTful 的框架，主要设计灵感来源于 tornado、sinatra 和 flask 这三个框架，但是结合了 Go 本身的一些特性（interface、struct 嵌入等）而设计的一个框架。

#### 架构

Beego有八个独立的模块 , 且高度解耦 . 可以不使用HTTP逻辑 , 也可以使用这些独立的模块 . 例如 , 使用cache模块做缓存逻辑 . 使用日志模块记录操作信息 . 使用config模块来解析各种格式的文件等等 .

* cache
* config
* context
* httplibs
* logs
* orm
* session
* toolbox

#### 执行逻辑

**MVC框架**

![](/assets/zhixingluoji.png)

