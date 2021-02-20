# GOMAXPROCS调整并发的运行性能

在 Go语言程序运行时\(runtime\)实现了一个小型的任务调度器 . 这套调度器的工作原理类似于操作系统调度线程 , Go程序调度器可以高效地将CPU资源分配给每一个任务 . 传统逻辑中 , 开发者需要维护线程池中线程与CPU核心数量的对应关系 . 同样的Go语言中也可以通过runtime.GOMAXPROCS\(\)函数做到 , 格式为 : 

```
runtime.GOMAXPROCS(逻辑CPU数量)
```

这里的逻辑CPU数量可以有如下几种数值 : 

* &lt;1 : 不修改任何数值
* =1 : 单核心执行
* &gt;1 : 多核并发执行

一般情况下 , 可以使用 runtime.NumCPU\(\) 查询 CPU 数量 , 并使用 runtime.GOMAXPROCS\(\) 函数进行设置 , 例如 : 

```
runtime.GOMAXPROCS(runtime.NumCPU())
```

Go 1.5 版本之前 , 默认使用的是单核心执行 . 从 Go 1.5 版本开始 , 默认执行上面语句以便让代码并发执行 , 最大效率地利用 CPU . 



