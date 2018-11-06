### 高性能IO

#### reactor
* 最早一个线程用，阻塞，无法并发
* 多线程，可并发，资源占用高，线程粒度大，要处理连接，读取和写入
* reactor模式
* 餐厅举例：
  * 多线程：一个服务员服务一个客人，点菜(读取)， 吃饭(处理)，埋单(写入)
  * reactor：一个服务员服务多个客人，事件驱动，当客人点菜的时候，服务员可以去服务其他客人，客人点完菜了，发送一个事件，叫来一个空闲的服务员。。。。

* reactor需要依赖操作系统提供的selector系统调用(select, poll, epoll)
* [参考资料1](https://www.cnblogs.com/doit8791/p/7461479.html)
* [参考资料2](https://blog.csdn.net/pistolove/article/details/53152708)


#### select，poll，epoll

* select，poll，epoll都是IO多路复用的机制
* select, poll缺点
	* 每次调用select，都需要把fd集合从用户态拷贝到内核态，这个开销在fd很多时会很大
	* 同时每次调用select都需要在内核遍历传递进来的所有fd，这个开销在fd很多时也很大
	* select支持的文件描述符数量太小了，默认是1024
* epoll如何解决这些问题
	* fd只拷贝一次，在注册时拷贝
	* 为每个fd注册一个回调，事件发生时把对应的fd加入就绪列表
	* 数量为最大可打开的文件数
* [参考资料](https://www.cnblogs.com/Anker/p/3265058.html)


### 服务化

#### 微服务(microservice)
* [微服务基础理论](https://medium.com/@Alibaba_Cloud/conways-law-a-theoretical-basis-for-the-microservice-architecture-c666f7fcc66a)
* [微服务系统设计原则](https://techbeacon.com/5-fundamentals-successful-microservice-design)
	* 合理地拆分服务
	* 如何定义接口
	* 负载均衡
	* 服务装载与卸载
	* 服务监控
* 避免分布式事务

#### 负载均衡(load balance)
* DNS [Round-robin DNS](https://en.wikipedia.org/wiki/Round-robin_DNS)
	* 不管服务是否正常
* zookeeper
	* rpc load-balance([dubbo](https://github.com/apache/incubator-dubbo), [grpc](https://github.com/makdharma/grpc-zookeeper-lb))
* [nginx](https://www.nginx.com/)
	* nginx+, [nginx的UI](https://demo.nginx.com/)
* [fabio](https://github.com/fabiolb/fabio)
	* go语言写的
	* TCP/HTTP
* [Haproxy](http://www.haproxy.org/)
	* TCP/HTTP
* [Træfik](https://github.com/containous/traefik/)
	* 最新的
	* 1.7w stars
	* 支持多容器
	* 支持http2, grpc
* LVS
	* 多服务器的负载均衡
	* 网络层
	* 高性能，高可用的服务器集群技术
	* 通过控制IP来实现负载均衡
	* VIP
	* [LVS简介及使用](https://www.cnblogs.com/codebean/archive/2011/07/25/2116043.html)
    * [keepalived](https://www.jianshu.com/p/b050d8861fc1)

#### 服务网格(Service Mesh)
* [什么是服务网格](https://www.nginx.com/blog/what-is-a-service-mesh/)
	* [什么是服务网格？为什么你需要它？](https://blog.csdn.net/gBbQRglVIr3dYi82/article/details/78936951)
	* [Service Mesh服务网格：是什么和为什么](https://blog.csdn.net/zyqduron/article/details/80433995)
	* 让服务治理变得简单
* [istio](https://istio.io/docs/)
	* [官方文档](https://istio.io/docs/)
	* [in codelabs](https://codelabs.developers.google.com/codelabs/cloud-hello-istio/#0)
	* [为什么需要istio](https://jimmysong.io/posts/why-do-we-need-istio/)
* [linkerd](https://github.com/linkerd/linkerd)
	* [官方文档](https://linkerd.io/docs/)
	* [聊聊Service Mesh：linkerd](https://blog.csdn.net/zl1zl2zl3/article/details/78678460?locationNum=2&fps=1)
* [envoy](https://github.com/envoyproxy/envoy)
	* [官方文档](https://www.envoyproxy.io/docs/envoy/latest/)
	* [Envoy和同类系统的对比](https://www.jianshu.com/p/e5655e9ce7fe)



#### 无服务(serverLess)
* 事件驱动
* [Faas介绍](http://dockone.io/article/2669)
* [IaaS，PaaS，SaaS 的区别](http://www.ruanyifeng.com/blog/2017/07/iaas-paas-saas.html)
* [几个开源faas 框架](https://www.cnblogs.com/rongfengliang/p/7746141.html)
* [从IaaS到FaaS—— Serverless架构的前世今生](https://aws.amazon.com/cn/blogs/china/iaas-faas-serverless/)
* [FaaS，未来的后端服务开发之道](https://www.jianshu.com/p/6e86c42f85bd)
* [What Is Function-as-a-Service](https://stackify.com/function-as-a-service-serverless-architecture/)
* 函数之间的调用和管理是一个棘手的问题

#### 服务间调用(RPC)
* 安全传输协议[TLS](https://blog.csdn.net/lizewenhh/article/details/79740609)
    * SSL1.0是SSL3.0的升级版，相当于SSL3.1
	* [SSL与TLS的区别以及介绍](https://blog.csdn.net/anningzhu/article/details/77517432)
    * [SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
* [dubbo](https://github.com/apache/incubator-dubbo)
	* java
* [grpc](https://github.com/grpc/grpc)
	* 跨语言
* [ice](https://github.com/zeroc-ice/ice)
	* 跨语言(不支持go)
	* [高性能](https://blog.csdn.net/whzhaochao/article/details/51406539)
* [thrift](https://github.com/apache/thrift)
	* 跨语言

#### 序列化
* [protobuf](https://github.com/protocolbuffers/protobuf)
* [thrift](https://github.com/apache/thrift)
* [avro](https://github.com/apache/avro)
* [ice](https://github.com/zeroc-ice/ice)

#### 分布式事务

* 原子性(Atomicity )、一致性( Consistency )、隔离性或独立性( Isolation)和持久性(Durabilily)，简称就是ACID
* CAP定理: 一致性(Consistency)、可用性(Availability)、分区容错性(Partition tolerance) 无法同时达到。
* BASE理论：
	* Basically Available（基本可用）
	* Soft state（软状态）
	* Eventually consistent（最终一致性）
* 两阶段提交（2PC）
	* 协调者、参与者
	* 协调者宕机会影响整个集群
	* 事务过程中所有参与者需要同步阻塞
	* 数据没有强一致，当协调者发出commit通知，而参与者未收到时会一直阻塞
* 三阶段提交
	* 投票、预提交、提交
	* 超时机制
	* 没有强一致性
* 补偿事务（TCC）
	* try, confirm, cancel
	* 数据一致性差，confirm和cancel都可能失败
* 本地消息表（异步确保）
	* 业界使用最多(ebay)
	* 添加本地消息表
	* 消息和业务一起加入本地事务
	* 消息通过MQ发送给事务另一端(消费者)
	* 消费者通知生产者成功或失败(通过MQ或者直接远程调用)
	* 生产者定期扫描消息表，处理未完成的消息
* MQ 事务消息
	* 类似两阶段提交的实现
	* 保证消息发送与本地事务同时成功或同时失败
	* RocketMQ支持事务消息
	* 主流的MQ不支持，RabbitMQ, kafka
* Sagas 事务模型（略）
* [参考资料](https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html)

### 分布式数据一致性
* [Paxos](https://zh.wikipedia.org/zh-hans/Paxos%E7%AE%97%E6%B3%95)（复杂难理解）
* [Raft](https://www.jianshu.com/p/138b4d267084)

### TLA+
* [home](https://lamport.azurewebsites.net/tla/tla.html)
* [验证程序在并发情况下的正确性](https://www.jianshu.com/p/7ae049ce4a82)
* [toolBox](http://lamport.azurewebsites.net/tla/toolbox.html)

### jvm工具
* jps jvm中的ps命令
* jstack 打印堆栈信息
* jstat 时统计堆内存使用情况
* jmap 打印java进程的内存中的所有对象情况
* jinfo 输出并修改运行时的java 进程的opts
* jconsole 一个java GUI监视工具
* [jvisualvm](https://blog.csdn.net/tzs_1041218129/article/details/59165488) 可视化工具
* jhat 离线分析heap
* [jvm-tools](https://github.com/aragozin/jvm-tools)
* [jvm排查工具箱jvm-tools](https://www.jianshu.com/p/846cfb217acf)
* [JVM 性能调优监控工具 jps、jstack、jmap、jhat、jstat 等使用详解](https://juejin.im/entry/58c53e4c128fe1006b3b2b1f)

### 数据存储

#### 关系型
* mysql
* postgresql

#### KV型
* redis

#### 乐观锁、悲观锁
* 乐观锁: 认为并发不会带来问题，一开始不拿锁，允许并发，如果失败了再重试或者回滚
    * CAS(Compare and Swap 比较并交换), 更新前先比较
    * 表中添加version字段，更新时判断version字段是否和之前取到的一致，不一致则重试或回滚

    ```
    select version, data from t_table;
    update t_table set data = #{data}, version = version + 1 where version = #{version};
    ```
    * 如果经常失败则性能差
* 悲观锁: 认为并发会带来问题，一开始就拿锁，不允许并发

#### 数据库索引实现方式
[Mysql索引实现](https://www.cnblogs.com/bonelee/p/6225211.html)
[MySQL的InnoDB索引原理详解](https://www.cnblogs.com/shijingxiang/articles/4743324.html)

* [AVL-Tree, B-Tree, B+Tree](https://www.cnblogs.com/vianzhang/p/7922426.html)
	* AVL：平衡二叉树
* MyISAM
    * B+Tree
    * 叶节点的data域存数据记录的地址
    * 可以没有主键
    * 索引文件和数据文件分开存储
* InnoDB
    * B+Tree
    * 必须有主键，如果没指定会有隐藏主键
    * 数据按主键聚集
    * 主键叶子节点保存了数据本身
    * 主键索引十分高效
    * 辅助索引的data域是主键
    * 索引文件包含了数据

#### mysql事务、并发、锁机制
* [这篇文章讲得很详细](https://blog.csdn.net/canot/article/details/53815294)
* 行级锁、表级锁、页级锁
  * 表级锁: 锁定整个表(MyISAM)，加锁快、易冲突、低并发
  * 页级锁: 锁定一个数据块(BDB)
  * 行级锁: 锁定索引到的行(InnoDB)，加锁慢、不易冲突、高并发
* InnoBD中的两种行级锁:
  * 共享锁: ...lock in share mode  允许一个事务去读，阻止其他事务获得相同数据集的排他锁。什么意思：
  	* 可以读
  	* 不能加排他锁
  * 排他锁: ...for update 允许获得排他锁的事务更新数据，阻止其他事务取得相同数据集的共享读锁和排他写锁。什么意思：
  	* 可以读
  	* 不能加共享锁
* [InnoDB事务执行流程](https://www.linuxidc.com/Linux/2018-04/152080.htm)

####　MYSQL其他

[视图](https://www.cnblogs.com/geaozhang/p/6792369.html)

[主从原理](https://www.jb51.net/article/136828.htm)




### 秒杀系统设计方法

* [电商,秒杀系统,设计思路和实现方法](https://blog.csdn.net/bigtree_3721/article/details/72760538)
* [淘宝大秒系统设计详解](https://blog.csdn.net/universe_ant/article/details/74375884)

### HTTP2
* [HTTP/2 新特性总结](https://www.jianshu.com/p/67c541a421f9)
* [HTTP2.0与HTTP1.0的区别](https://blog.csdn.net/u012657197/article/details/77877840)

### 消息队列
#### rabbitMQ
* [AMQP协议](http://langyu.iteye.com/blog/759663/)
* Exchanges: 接收生产者发送的消息
	* [Exchanges三种模式详解](https://blog.csdn.net/fxq8866/article/details/62049393)
	* Direct: Queue需要bind到Exchange上并要求一个routing key，完全匹配routing key的消息会转发到Queue。
	* 消息发送到与Exchange bind的所有Queue上
	* Topic: 和Direct的区别是，routing key采用模式匹配
* Queue: 生产者发的消息最终达到这里
* Bindings: 决定消息如何路由到正确的Queue
* Routing key: 消息路由到Queue时的关键词
* [Ack](https://www.dev-heaven.com/posts/36563.html): 消息确认，默认为自动确认，server端不必等待consumer端确认，就丢弃消息。开启手动确认后，server端等待consumer确认之后才会丢弃消息。如果consumer未发送ack,则server通过consumer的连接是中断来确认消息是否可以重新发给的其他的consumer。
* [事务和Confirm](https://baijiahao.baidu.com/s?id=1604438957587131228&wfr=spider&for=pc): 为了解决broker到publisher的确认，默认不开启。

[Ack and Confirm](http://www.rabbitmq.com/confirms.html)

#### kafka
* Topic: 消息按topic组织，生产者向topic发送消息，消息费订单topic的消息
* Partition: 一个topic可以有多个partition, 消息分散在partition中
* Consumer Group: 一个group内可以有多个consumer, 一个group有一个offset。一个topic可以有多个group, 每个group管理自己的offset
* [Kafka用zk做什么](https://blog.csdn.net/m0_37738114/article/details/80406948)
* offset管理: kafka不马上删除数据，而是通过更新offset。

#### redis

### 容器技术

### 函数式编程(FP)
* 更数学的思维方式
* 更安全的并发支持
* [为什么学习haskell](https://www.jianshu.com/p/b52cea578324)
    * 强类型，强迫写出逻辑严密的代码(代码能编译通过，逻辑基本没问题)
    * 代码即文档
* scheme语言概要[上](https://www.ibm.com/developerworks/cn/linux/l-schm/index1.html), [下](https://www.ibm.com/developerworks/cn/linux/l-schm/index2.html)
* [48小时写一个Scheme解析器](https://en.wikibooks.org/wiki/Write_Yourself_a_Scheme_in_48_Hours/Parsing)
* [haskell趣学指南](https://legacy.gitbook.com/book/mno2/learnyouahaskell-zh/details)
* [WHAT I WISH I KNEW WHEN LEARNING HASKELL](http://dev.stephendiehl.com/hask/)
* [stack guide](https://docs.haskellstack.org/en/stable/GUIDE/)
* [haskell学习资料大全](https://github.com/bitemyapp/learnhaskell/blob/master/guide-zh_CN.md)

### VOIP
#### SIP

* [语音业务VOIP开发之SIP协议篇：SIP基本场景分析](https://blog.csdn.net/zqixiao_09/article/details/79519335)
* [语音业务VOIP开发之SIP协议篇（二） —— SIP报文浅析](https://blog.csdn.net/zqixiao_09/article/details/79519575)
* [代理（Proxy）和背靠背用户代理（B2BUA）](https://blog.csdn.net/livingpark/article/details/7088659)
* [freeswitch中文文档](http://www.freeswitch.org.cn/2010/04/30/freeswitch-zhong-wen-wen-dang.html)

#### FREESWITCH

[语音业务VOIP开发之SIP协议篇：SIP报文浅析](https://blog.csdn.net/zqixiao_09/article/details/79519575)

#### SIPXECS

[sipxecs总体介绍](https://www.xuebuyuan.com/684099.html)

#### OPENSIPS

[opensips介绍](https://www.xuebuyuan.com/1900947.html)

#### Asterisk

[开源软交换系统 FreeSwitch 与 Asterisk 比较](https://www.cnblogs.com/welhzh/p/5650443.html)
#### FREESWITCH应用优化

* [FreeSWITCH折腾笔记8——使用OpenSIPS进行负载均衡](http://blog.51cto.com/908405/2235934)
* [freeswitch 之mysql性能优化篇](https://blog.csdn.net/swcxy12315/article/details/79464931)
* [freeswitch使用mysql代替sqlite以及通过lua管理用户登录](https://blog.csdn.net/thrill008/article/details/78413260)
### 计算机网络相关

#### NAT相关

[单IP做NAT支持的最大连接数问题](https://blog.csdn.net/qiushanjushi/article/details/43306511)


### HTTP相关

#### 跨域
[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

### C编译相关
#### configure, make, make install
* [The magic behind configure, make, make install](https://robots.thoughtbot.com/the-magic-behind-configure-make-make-install)
* [Using Autotools](https://developer.gnome.org/anjuta-build-tutorial/stable/create-autotools.html.en)
* [configure.ac and configure.in](https://stackoverflow.com/questions/3782994/any-difference-between-configure-ac-and-configure-in-and-makefile-am-and-makefi)
* [automake cmake](https://blog.csdn.net/cnsword/article/details/7542696)


### 加密相关
[MD5算法原理](https://www.cnblogs.com/ttss/p/4243274.html)
[PKCS5 PKCS7](https://blog.csdn.net/test1280/article/details/75268255)


### 前后端分享API文档生成工具
* [swagger](https://swagger.io/)
* [rap2-delos](https://github.com/thx/rap2-delos)
* [rap](https://github.com/thx/RAP)
* [easy-mock](https://github.com/easy-mock/easy-mock/blob/dev/README.zh-CN.md)


### nodejs 工具
* [nodejs进程管理工具-PM2](https://github.com/Unitech/pm2)

### 网络抓包相关
[tcpdump抓包对性能的影响](https://blog.csdn.net/dog250/article/details/52502623<Paste>)
