spring cloud和dubbo的区别
- spring cloud社区活跃度比dubbo要高，dubbo只实现了服务治理，而spring cloud则包含了微服务架构的方方面面，服务治理只是其中之一。
| |Dubbo|Spring Cloud|
|服务注册中心|Zookeeper/Redis|Spring Cloud netflix Eureka|
|服务调用方式|RPC|REST API|
|服务网关|none|Spring Cloud Netflix Zuul|
|断路器|none|Spring Cloud Netflix Hystrix|
|分布式配置|none|Spring Cloud Config|
|服务跟踪|none|Spring Cloud Sleuth|
|消息总线|none|Spring Cloud Bus|
|数据流|none|Spring Cloud Stream|
|批量任务|none|Spring Cloud Task|

最大的区别：Dubbo使用RPC调用服务，Spring Cloud使用REST API。


Java并发编程部分
并发包，即java.util.conncurrent及其子包，具体包括：
- 提供比synchronized更加高级的各种同步结构，包括CountDownLatch,CyclicBarrier,Semaphore；可以利用Semaphore作为资源控制器，限制同时进行工作的线程数量。
- 各种线程安全的容器，比如ConcurrentHashMap，有序的ConcurrentSkipListMap，或者通过类似快照机制，实现线程安全的动态数组CopyOnWriteArrayList等。
- 各种并发队列的实现，如各种BlockingQueue的实现，比较典型的ArrayBlockingQueue,SynchorousQueue或针对特定场景的PriorityBlockinngQueue等。
- Excutor框架，可以创建各种不同类型的线程池，调度任务运行等，绝大部分情况下，不再需要自己从头实现线程池和任务调度器。
对于同步结构：
+ CountDownLatch:允许一个或多个线程等待某些操作完成。
+ Cyclicbarrier:一种辅助性的同步结构，允许多个线程等待达到某个屏障。
+ Semaphore:Java版本的信号量实现。
Semaphore通过控制一定数量的permit的方式，达到限制通用资源访问的目的。