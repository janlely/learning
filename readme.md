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
	* [SSL与TLS的区别以及介绍](https://blog.csdn.net/anningzhu/article/details/77517432) 
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


### HTTP2
[HTTP/2 新特性总结](https://www.jianshu.com/p/67c541a421f9)
[HTTP2.0与HTTP1.0的区别](https://blog.csdn.net/u012657197/article/details/77877840)
