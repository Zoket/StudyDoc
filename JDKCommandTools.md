### JDK命令行工具

##### jps：虚拟机进程状况工具

和linux的ps命令类似，列出正在运行的虚拟机进程，并显示虚拟机执行主类（main()函数所在的类）的名称，以及这些进程的LVMID（本地虚拟机唯一ID）。

对于本地虚拟机进程来说，LVMID和PID是一致的。

jps命令格式：

`jps [options] [hostid]`

hostid为RMI注册表中注册的主机名。options选项见下表：

| 选项 | 作用                                               |
| ---- | -------------------------------------------------- |
| -q   | 只输出LVMID，省略主类名称                          |
| -m   | 输出虚拟机进程启动时传递给主类main()函数的参数     |
| -l   | 输出主类的全名，如果进程执行的是jat包，输出jar路径 |
| -v   | 输出虚拟机进程启动时的JVM参数                      |



##### jstat：虚拟机统计信息监视工具

监视虚拟机各种运行状态信息，显示本地或远程虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。作为运行期定位虚拟机性能问题的首选工具。

jstat命令格式：

`jstat [option vmid [interval[s|ms] [count]]]`

interval参数时查询间隔，count代表查询次数。

其中的VMID，如果是本地虚拟机进程，和LVMID一致，如果是远程虚拟机进程，则VMID格式为：

`[protocol:][//]lvmid[@hostname[:port]/servername]`

option选项主要分3类：类装载、垃圾收集和运行期编译状况，具体见下表：

| 选项              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| -class            | 监视类装载、卸载数量、总空间及类装载的耗费时间               |
| -gc               | 监视Java堆状况，包括Eden区、2个Survivor区、老年代、永久代等的容量、已用空间、GC时间合计等信息 |
| -gccapacity       | 监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大和最小空间 |
| -gcutil           | 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比 |
| -gccause          | 与-gcutil功能一样，但会额外输出导致上一次GC产生的原因        |
| -gcnew            | 监视新生代GC的状况                                           |
| -gcnewcapacity    | 监视内容与-gcnew基本相同，输出主要关注使用到的最大和最小空间 |
| -gcold            | 监视老年代GC的状况                                           |
| -gcoldcapacity    | 监视内容与-gcold基本相同，输出主要关注使用到的最大和最小空间 |
| -gcpermcapacity   | 输出永久带使用到的最大和最小空间                             |
| -compiler         | 输出JIT编译器编译过的方法、耗时等信息                        |
| -printcompilation | 输出已经被JIT编译的方法                                      |



##### jinfo：Java配置信息工具

实时查看和调整虚拟机的各项参数。jps -v命令可以查看虚拟机启动时显式指定的参数列表，但是对于默认值参数，只能通过jinfo -flag进行查询。使用jinfo -flag [+|-]name 或 -flag name=value可以修改一部分运行期可写的虚拟机参数值。

jinfo命令格式：

`jinfo [option] pid`

option参数选项见下表：

| 选项      | 作用                                             |
| --------- | ------------------------------------------------ |
| -flag     | 查询虚拟机                                       |
| -sysprops | 把虚拟机进程的System.getProperties()内容打印出来 |



##### jmap：Java内存映像工具

一般用于生成堆转储快照，即heapdump。

- 获取dump文件的几种方式：
  1. 使用jmap
  2. -XX:+HeapDumpOnOutOfMemoryError参数，在OOM异常之后自动生成dump文件
  3. -XX:HeapDumpOnCtrlBreak参数，使用Ctrl+Break组合键生成
  4. Linux系统下kill -3命令发送进程退出信号给虚拟机进程

jmap命令格式：

`jmap [option] vmid`

option选项见下表：

| 选项           | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| -dump          | 生成Java堆转储快照，格式为`-dump:[live,]format=b,file=<filename>`，其中live子参数说明是否只dump出存活的对象 |
| -finalizerinfo | 显示在F-Queue中等待Finalizer线程执行finalize方法的对象       |
| -heap          | 显示Java堆详细信息，如使用哪种回收器、参数配置、分代状况等   |
| -histo         | 显示堆中对象统计信息，包括类、实例数量和合计容量             |
| -permstat      | 以ClassLoader为统计口径显示永久代内存状态                    |
| -F             | 当虚拟机进程堆-dump选项没有相应时，可使用此选项强制生成dump  |



##### jhat：虚拟机堆转储快照分析工具

和jmap搭配使用，分析其生成的dump文件。jhat内置微型http、html服务器，生成结果后可以在浏览器中查看。命令格式：`jhat filename`，当提示“Server is ready”，浏览器访问 http://localhost:7000/就可以看到分析结果。



##### jstack：java堆栈跟踪工具

生成虚拟机当前时刻的线程快照（threaddump或javacore文件），即当前虚拟机内每一条线程正在执行的方法堆栈的集合。生成线程快照的主要目的是定位线程出现长时间停顿的原因。如线程间死锁、死循坏、请求外部资源导致长时间等待的时候，通过jstack查看各个线程的调用堆栈，就可以知道没有响应的线程在后台做什么事情，或等待什么资源。

jstack命令格式：

`jstack [option] vmid`

option选项见下表：

| 选项 | 作用                                         |
| ---- | -------------------------------------------- |
| -F   | 当正常输出的请求不被响应时，强制输出线程堆栈 |
| -l   | 除堆栈外，显示关于锁的附加信息               |
| -m   | 如果调用到本地方法的话，可以显示C/C++的堆栈  |

