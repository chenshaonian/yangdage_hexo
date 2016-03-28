---
title: 初识NSQ分布式实时消息架构 
---
### 1.NSQ知识普及

NSQ是一个基于Go语言的开源的分布式实时消息平台，他的代码托管在GitHub上。

```
https://github.com/nsqio/nsq
```

NSQ可用于大规模系统的实时消息服务，它的设计目标是为在分布式环境下提供一个强大的去除中心化的分布式服务架构，可以每天处理数以亿计的实时消息。NSQ的优点是无单点故障、故障容错、高可用性和信息传递的高可靠性。NSQ安装部署简单，容易水平扩展，目前已有很多公司都是采用其作为自身企业内部的实时消息服务。而且它的灵活性很强，支持很多种协议。官方直接提供了拆箱可用的Go库和Python库。好，其他的就不废话了，随便搜索下NSQ相关的博文或者去NSQ的官方网站了解更详细的信息吧。

### 2.NSQ的四大组件

nsqlookupd：管理nsqd节点拓扑信息并提供最终一致性的发现服务的守护进程

nsqd：负责接收、排队、转发消息到客户端的守护进程，并且定时向nsqlookupd服务发送心跳

nsqadmin：nsq的web统计界面，可实时查看集群的统计数据和执行一些管理任务 

utilities：常见基础功能、数据流处理工具，如nsq_stat、nsq_tail、nsq_to_file、nsq_to_http、nsq_to_nsq、to_nsq 

### 3.NSQ依赖

#### 3.1 GO：NSQ是有GO语言开发的，必不可少的需要事先安装配置好GO相关的环境变量信息。

（i）首先自然是去官网找到对应系统的较新较稳定的版本安装包，中文站地址：http://golangtc.com/download

由于其官方地址在国外，下载速度可能比较慢，推荐使用本地下载的链接进行下载。

```bash
wget http://golangtc.com/static/go/go1.5beta1.linux-amd64.tar.gz
```

（ii）下载完安装包后解压并且复制到/usr/local/目录下

```bash
tar zxvf go1.5beta1.linux-amd64.tar.gz

mv go /usr/local
```

（iii）GO环境变量配置

打开环境变量配置文件

```bash
sudo vim /etc/profile
```

然后在文件的最后追加以下配置

```bash
export GOROOT=/usr/local/go
export GOPATH=\$GOROOT/bin
export PATH=\$PATH:\$GOPATH
```

配置好环境变量后再让它马上生效

```bash
source /etc/profile
```

最后查看是否安装成功

```bash
go version
```

如果控制台打印出了对应的版本号信息，那就恭喜你了，等等。。。别急，这才刚刚开始。

#### 3.2 GIT：因为稍候需要从github上获取NSQ的安装文件

直接命令安装就好

```bash
yum install git
```

### 4.安装NSQ

命令行直接执行下面的命令安装NSQ

```bash
go get github.com/kr/godep
go get github.com/bmizerany/assert
godep get github.com/bitly/nsq/...
```

**注：godev为GO的语言依赖管理包，assert为GO的测试框架，至于最后一个嘛，自己猜吧**

到此，NSQ就安装好了，接下来就可以一起愉快的玩耍了

### 5.启动部署和测试

这节的内容就直接翻译了官网的原文了，有兴趣的可以自己去看下

http://nsq.io/overview/quick_start.html

#### 5.1 打开一个终端，启动nsqlookupd

```bash
nsqlookupd
```

#### 5.2 再打开一个终端，启动nsq

```bash
nsqd --lookupd-tcp-address=127.0.0.1:4160
```
#### 5.3 再打开一个终端，启动nsqadmin

```bash
nsqadmin --lookupd-http-address=127.0.0.1:4161
```
#### 5.4 创建一个topic并且发布一条消息，由于nsq支持http协议发布消息，我们可以直接使用curl命令。

```bash
curl -d 'hello world 1' 'http://127.0.0.1:4151/put?topic=test'
```

#### 5.5 再开一个终端，@~@!!!,开了好多终端了，来处理消息数据流，将消息写入/tmp文件加下的日志文件,文件名默认由主题topic+主机+日期时间戳组成。

```bash
nsq_to_file --topic=test --output-dir=/tmp --lookupd-http-address=127.0.0.1:4161
```
#### 5.6 我们再发布两条消息，看看日志文件中的变化。

```bash
curl -d 'hello world 2' 'http://127.0.0.1:4151/put?topic=test'
curl -d 'hello world 3' 'http://127.0.0.1:4151/put?topic=test'
```
#### 5.7特别说明
需要特别注意的是这里的nsq_to_file客户端进程并不确定消息什么时候会发布，所以它会在连接没有断开的情况下定时去nsqlookupd中检索消息，确保消息不丢失。

![这里写图片描述](http://img.blog.csdn.net/20150830214444426)

![这里写图片描述](http://img.blog.csdn.net/20150830214529061)

![这里写图片描述](http://img.blog.csdn.net/20150830214544464)

### 6.nsqadmin的web统计
![这里写图片描述](http://img.blog.csdn.net/20150830214340984)

nsq自身提供了一个便于查看和操作的web界面，可以实时查看统计信息和执行一些管理任务。

#### 6.1浏览器打开nsqadmin的web页

```bash
http://192.168.1.111:4171/
```

由于默认情况下linux系统，nsqadmin的默认端口4171限制了对外访问，所以我们要打开4171端口的路由访问限制，便于外部系统的浏览器访问。

```bash
iptables -A  INPUT -p tcp --dport 4171 -j ACCEPT
```

- 参数讲解：
–A 添加一条规则的意思
–p 指定是什么协议，这里是tcp协议
–dport 就是目标端口，当数据从外部进入服务器为目标端口
–sport 数据从服务器出去，则为数据源端口使用 
–j 就是指定是 ACCEPT -接收 或者 DROP 不接收

#### 6.2 Topics界面
![这里写图片描述](http://img.blog.csdn.net/20150830213809966)

这里详细的列出了所有的发布的topic主题列表，可以点进去查看单个主题的详细统计信息。还可以管理主题，比如清空主题的消息队列，删除主题，暂停主题等等。

#### 6.3 Nodes节点列表
![这里写图片描述](http://img.blog.csdn.net/20150830213848425)

这个页面可以查看当前nsqlookupd协调管理的节点列表，以及一些连接信息，比如端口，连接数，主题和主题数量等等。

#### 6.4 Counter消息计数页
![这里写图片描述](http://img.blog.csdn.net/20150830214013450)

这个页面最简洁，中间一个硕大的计数牌，展示已经处理过的消息数目。

#### 6.5 Lookup界面
![这里写图片描述](http://img.blog.csdn.net/20150830214233531)

这里我们可以创建消息主题和消息通道。
