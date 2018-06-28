
## 一、JVM知识价值
说到JVM，不少java码农会不由自主产生一种仰视，觉得高深莫测。如果对JVM有一定的了解，那么这个java码农已经是码农中的高级货。如果对JVM知识有所了解和实战，那么在面试中通过面试的概率大大增加，笔者也因为这方面知识在面试中屡试不爽。这就是JVM知识的价值和魅力。


## 二、JVM知识积累
不过JVM知识如何来，不同于普通的编程知识，JVM理论知识很多，实战案例非常少，系统化实战更好。而计算机是一门实战科学，没有实战的知识很容易遗忘，所以很多的码农在JVM方面一直没有入门。
因为JVM异常大部分需要高并发、高流量下发生，这种场景一般都在大公司发生，所以掌握JVM知识的往往是大公司的技术人员；特别是大公司的JVM团队或者基础架构团队，经常碰到case，知识就不断积累。
那么对于普通开发是否有机会深入学习到这方面知识，本文把各类复杂的场景简化成简单的例子，大家可以自己动手不断分析和学习。

## 三、理论知识
没有理论知识，就不知道问题根源，所以理论知识是必要的，我这边就给两个图，大家可以网上学习相关知识：
![image](https://github.com/sesamegu/doc/blob/master/tmp/JMM.png?raw=true)
![image](https://github.com/sesamegu/doc/blob/master/tmp/J8.png?raw=true)

## 四、工具
工欲善其事，必先利其器。这里简单介绍JVM领域必备工具，具体的工具用法在第五部分会涉及<br/>
a，jstack   jdk 自带工具，用来dump JVM的堆栈 <br/>
b，jstat jdk 自带工具，用来查询GC的情况等<br/>
c，jmap jdk 自带工具  用来dump heap和实时查看heap中的内容<br/>
d，MAT  Eclipse 提供的分析JVM的Heap文件<br/>
e，BTrace 开源软件，Byte Trace，分析问题的神器<br/>

## 五、跟着例子学习
我这边给出最常见的6个JVM的异常，具体参见下图：
![image](https://github.com/sesamegu/doc/blob/master/tmp/企业微信截图_fbf7b5e0-ca49-41df-92a7-775ce765878a.png?raw=true)
* 1，Heap OOM
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/HeapOomPractice.java <br/>
分析：通过Mat分析内存 <br/>
解决：不使用的对象需要及时释放引用，没有引用的对象能被GC回收 <br/>
常见原因：在集合类中存放大量对象 <br/>

* 2，Meta OOM
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/MetaOomPractice.java <br/>
分析：通过Mat分析内存；也可以用BTrace <br/>
解决：不合理的创建了大量的class，且不能被回收； <br/>
常见原因：java调用动态语言如groovy时容易采坑；用class想看的类库cglib、asm动态创建了大量类； <br/>

* 3，Native OOM
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/NativeOomPractice.java <br/>
分析：btrace追踪ByteBuffer;Google perftools工具  <br/>
解决：及时释放不使用的native内存 <br/>
常见原因：使用了ByteBuffer的direct功能或者通过JNI使用本地C++类库 <br/>

* 4，DeadLock
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/DeadLockPractice.java <br/>
分析：用jstack分析线程堆栈，有环形等待锁 <br/>
解决：打破死锁的一个条件：锁有序 <br/>
常见原因：多处代码加锁顺序不一致 <br/>

* 5，CPU 100%
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/CpuHighPractice.java <br/>
分析：用top -p pid -H找到消费cpu的线程ID，在jstack中找到对于的线程ID，就能定位到cpu消耗在哪里 <br/>
解决：cpu100%很多时候是不合理的死循环，找到对应的逻辑进行调整 <br/>
常见原因：死循环 <br/>

* 6，StackOverflow
代码地址：https://github.com/sesamegu/snippets/blob/master/src/main/java/com/mikegu/tools/JVM/StackOverflowPractice.java <br/>
分析：java 线程的独有的栈溢出； <br/>
解决：找到为什么需要这么深的递归，解决之 <br/>
常见原因：深递归 <br/>

## 六、附件
工具的具体使用可以参见我的文档:
https://github.com/sesamegu/doc/tree/master/tmp/JVM%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E5%88%86%E6%9E%90%E5%92%8C%E5%B7%A5%E5%85%B7.zip

