# Beego的MVC架构

![](/assets/beegomvc.png)

框架执行文字描述 :

* 在监听的端口接收数据 , 默认监听在8080端口 . 
* 用户请求到达8080端口之后进入beego的处理逻辑 . 
* 初始化Context对象 , 根据请求判断是否为WebSocket请求 , 如果是的话设置Input , 同时判断请求的方法是否在标准请求方法中\(GET、POST、PUT、DELETE、PATCH、OPTIONS、HEAD\) , 防止用户的恶意伪造请求攻击造成不必要的影响 . 
* 执行 BeforeRouter 过滤器 , 当然在 beego 里面有开关设置 . 如果用户设置了过滤器 , 那么该开关打开 , 这样可以提高在没有开启过滤器的情况下提高执行效率 . 如果在执行过滤器过程中 , responseWriter 已经有数据输出了 , 那么就提前结束该请求 , 直接跳转到监控判断 . 



