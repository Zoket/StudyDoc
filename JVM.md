## JVM原理

**1. JVM运行时数据区**

1. PC寄存器

   用于存储每个线程下一步将要执行的JVM指令。

2. JVM栈

   **线程私有**，每个线程创建的同时都会创建JVM栈，其中存放当前线程

