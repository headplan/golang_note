# 安装

Go安装包的形式 :

```
go get github.com/astaxie/beego
```

常见问题 :

git 没有安装 .

git https 无法获取 , 可以关闭git的https验证 .

```
git config --global http.sslVerify false
```

默认安装在公共的GOPATH中 :

```
/Users/headplan/go
```

#### Beego的升级

Go方式升级\(推荐\)

```
go get -u github.com/astaxie/beego
```

源码覆盖方式升级

```
# 下载源码覆盖后执行
go install  github.com/astaxie/beego
```

## Bee工具

协助快速开发工具 , 用于项目的创建、热编译、开发、测试、和部署 .

#### 安装

```
go get github.com/beego/bee
```

Bee可执行文件默认存放在`$GOPATH/bin`里 , 所以需要把`$GOPATH/bin`添加到环境变量中 , 才可以进行下一步 . 

如果本机设置了`GOBIN`目录 , 上面的命令就会安装到`GOBIN`下 , 添加 GOBIN 到环境变量中即可 . 

