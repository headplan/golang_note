# Beego的MVC架构

![](/assets/beegomvc.png)

框架执行文字描述 : 

* 在监听的端口接收数据 , 默认监听在8080端口 . 
* 用户请求到达8080端口之后进入beego的处理逻辑 . 
* 初始化Context对象 , 根据请求判断是否为WebSocket请求 , 如果是的话设置Input , 同时判断请求的方法是否在标准请求方法中\(GET、POST、PUT、DELETE、PATCH、OPTIONS、HEAD\) , 防止用户的恶意伪造请求攻击造成不必要的影响 . 



