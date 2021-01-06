# 微服务概览与治理

### 单体架构

尽管也是模块化逻辑 , 但是最终它还是会打包并部署为单体式应用 . 其中最主要问题就是这个应用太复杂 , 以至于任何单个开发者都不可能搞懂它 . 应用无法扩展 , 可靠性很低 , 最终 , 敏捷性开发和部署变的无法完成 .

![](/assets/danti.png)

最终的对应思路 : 化繁为简 , 分而治之 .

![](/assets/fenerzhizhi.png)

### 微服务起源

大家经常谈论的是一个叫SOA\(面向服务的架构模式\) , 它和微服务又是什么关系 ? 你可以把微服务想成是 SOA 的一种实践 . 

> You should instead think of Microservices as a specific approach for SOA in the same way that XP or Scrum are specific approaches for Agile software development.

* **小即是美** : 小的服务代码少 , bug 也少 , 易测试 , 易维护 , 也更容易不断迭代完善的精致进而美妙 .
* **单一职责** : 一个服务也只需要做好一件事 , 专注才能做好 .
* **尽可能早地创建原型** : 尽可能早的提供服务 API , 建立服务契约 , 达成服务间沟通的一致性约定 , 至于实现和完善可以慢慢再做 .
* **可移植性比效率更重要** : 服务间的轻量级交互协议在效率和可移植性二者间 , 首要依然考虑兼容性和移植性 .



