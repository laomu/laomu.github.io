[TOC]

# 一、下载

## 1、 什么是`Zookeeper`

`Zookeeper` 是一个开源的、分布式的、为分布式应用提供协调服务的Apache开源项目。在项目开发中主要应用于`统一命名服务`、`统一配置管理`、`统一集群管理`、`服务节点管理`、`负载均衡`等，在处理数据时具有分区安全性、全局数据一致性、数据更新原子性等特性，在集权操作过程中通过半数原则管理选举机制，在集群中通过唯一的`Leader`管理所有的`Follower`集群节点，是一个非常流行的服务框架。

>   注意：官方介绍
>
>   ## What is ZooKeeper?
>
>   ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. All of these kinds of services are used in some form or another by distributed applications. Each time they are implemented there is a lot of work that goes into fixing the bugs and race conditions that are inevitable. Because of the difficulty of implementing these kinds of services, applications initially usually skimp on them, which make them brittle in the presence of change and difficult to manage. Even when done correctly, different implementations of these services lead to management complexity when the applications are deployed.

## 2、 下载

官方网站：`https://zookeeper.apache.org`

下载网址：`https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/`

下载对应平台的`Zookeeper`安装包。

![image-20190709222149253](../../FangCloudV2/%E5%A4%A7%E7%89%A7%E8%8E%AB%E9%82%AA/%E4%B8%AA%E4%BA%BA%E7%AC%94%E8%AE%B0/big%20data/assets/image-20190709222149253.png)



# 二、 安装

`Zookeeper`的安装方式区分为`单机模式`、`分布式`配置方式。

## 1、 单机模式

解压软件到指定目录`/software`，在解压后的`Zookeeper`根目录中创建文件夹`zkData/`用于存储持久化数据。

```shell
$ cd /software/zookeeper-3.4.14/
$ mkdir zkData
```

进入`Zookeeper`配置文件目录，修改启用配置文件

```shell
$ cd /software/zookeeper-3.4.14/conf
$ mv zoo_sample.cfg zoo.cfg
```

修改配置文件中的配置选项，指定数据存储目录

```properties
#dataDir=/tmp/zookeeper
dataDir=/software/zookeeper-3.4.14/zkData
```

默认情况下的数据存储目录是`/tmp/zookeeper`，我们指定自己的目录存储持久数据。

## 2、 分布式

 <TODO>



# 三、 启动、停止

## 1、 `zkServer.sh start`

启动`Zookeeper`可以直接使用提供在`bin/`中的命令进行操作：

```shell
$ zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /software/zookeeper-3.4.14/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

## 2、 `zkServer.sh status`

如此`Zookeeper`服务端已经启动完成，可以通过`zkServer.sh status`命令查看其进程状态

```shell
$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /software/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: standalone
```

如果提示`Mode: standalong` 表示当前`Zookeeper`是按照单机模式启动状态。

## 3、 `zkServer.sh stop`

停止`Zookeeper`服务，可以通过命令`zkServer.sh stop`停止当前服务端应用。

```shell
$ zkServer.sh stop
ZooKeeper JMX enabled by default
Using config: /software/zookeeper-3.4.14/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
```

## 4、 `zkCli.sh` 客户端

通过`zkCli.sh`命令，可以启动连接到服务器的`客户端`，通过客户端操作服务端上的数据。

```shell
$ zkCli.sh
Connecting to localhost:2181
......
[zk: localhost:2181(CONNECTED) 0] 
```

出现如上提示，说明客户端已经成功连接`Zookeeper`服务端，可以通过该交互窗口完成服务端数据的操作。

在客户端可以完成的操作命令如下：

```shell
[zk: localhost:2181(CONNECTED) 0] help   查看客户端操作命令
ZooKeeper -server host:port cmd args
	stat path [watch]
	set path data [version]
	ls path [watch]
	delquota [-n|-b] path
	ls2 path [watch]
	setAcl path acl
	setquota -n|-b val path
	history 
	redo cmdno
	printwatches on|off
	delete path [version]
	sync path
	listquota path
	rmr path
	get path [watch]
	create [-s] [-e] path data acl
	addauth scheme auth
	quit 
	getAcl path
	close 
	connect host:port
```

### （1）`help`

查看当前客户端交互过程中所有可操作命令及选项。

### （2）`ls path [watch]`

查看指定路径`path`下的子节点。

-   `watch`选项：监听该路径下的节点变化，一旦发生变动即可产生通知消息。

### （3）`ls2 path [watch]`

查看指定路径`path`下的详细节点信息。

-   `watch`选项：监听该路径下的节点变化，一旦发生变动即可产生通知消息

### （4）`create [-s] [-e] path data acl`

在`Zookeeper`服务端创建一个节点。

-   `-s` 选项：创建一个带序号的节点，序号由服务端程序计数器递增产生。
-   `-e` 选项：创建一个临时节点，临时节点会在客户端断开连接时自动删除。
-   `path`：创建的节点路径
-   `data`：创建的节点中存储的数据
-   `acl`：访问控制

### （5）`delete [version]` & `rmr`

`delete` 删除一个节点

`rmr` 递归删除节点

### （6）`set path data [version]`

给指定路径`path`的节点上设置数据`data`，多用于修改节点数据使用。

### （7）`get path [watch]`

获取指定`path`路径上节点的数据

-   `watch`选项：监听该节点上的数据变化，一旦数据发生变化即刻产生通知消息。

### （8）`quit`

客户端退出，断开和服务端之间的连接。



# 四、 `ZK集群`编码管理

```java
import org.apache.zookeeper.*;

import java.io.IOException;

/**
 * <p>项目文档： 自定义模拟服务器程序~在Zookeeper注册中心中注册信息</p>
 * <p>服务器上线~注册中心就会登录服务器信息，提供给客户端调用执行</p>
 * <p>服务器宕机离线，注册中心机会删除服务器的注册信息，客户端就会知晓该服务器已经离线，不再继续调用</p>
 * @author 大牧
 * @version V1.0
 */
public class DistrubutedServer {

    private ZooKeeper zooKeeper;
    private String zkUrl = "localhost:2181";
    private int sessionTimeout = 2000;

    public static void main(String[] args) throws IOException, KeeperException, InterruptedException {
        // 创建服务对象
        DistrubutedServer server = new DistrubutedServer();

        // 连接服务器
        server.connectServer();

        // 创建节点，注册服务
        server.register();

        // 业务处理
        server.business();

    }


    private void business() throws InterruptedException {
        // 模拟程序业务处理过程，进程正在执行不会退出
        Thread.sleep(Long.MAX_VALUE);
    }


    private void register() throws KeeperException, InterruptedException {
        // 连接Zookeeper服务创建节点
        zooKeeper.create("/servers/local",
                "localhost server".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL_SEQUENTIAL);
        System.out.println("节点创建完成");
    }


    private void connectServer() throws IOException {
        // 创建Zookeeper客户端连接对象
        zooKeeper = new ZooKeeper(zkUrl, sessionTimeout, (event)-> {
            // TODO 监听事件 回调操作
        });
    }

}
```

