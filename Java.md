#Java基础知识
##HashMap 与 ConcurrentHashMap
HashMap的扩容机制，ConcurrentHashMap的原理：
> [HashMap的设计原理和实现分析](HashMap的设计原理和实现分析 "http://blog.csdn.net/luanlouis/article/details/41576373")    
> HashMap造成的死锁问题    
> [ConcurrentHashMap原理分析](ConcurrentHashMap原理分析 "http://pengtyao.iteye.com/blog/1074271")

我们可以在单线程时使用HashMap提高效率，而多线程时用Hashtable来保证安全。

HashMap作为缓存导致的思索问题分析

##序列化的作用是，序列化id会出现哪些问题
[外观模式](http://www.itzhai.com/javascript-design-patterns-notes-combined-mode.html#2、外观模式： "外观模式")的例子：[Java序列化ID问题](http://www.ibm.com/developerworks/cn/java/j-lo-serial/#major3 "Java序列化ID问题")    
这是一个关于客户端程序版本升级的例子，当服务器端的对象改动，重新生成了id，这个时候客户端解析旧的对象就会失败，这个时候，必须重新从服务器端获取最新的对象序列化文件。

##JDK常用库，JDK源代码

###Java IO类库结构图，和所用到的设计模式

##NIO
###阻塞后的通知机制

##RuntimeException与CheckedException
**checked exception**    
checked exception是从java.lang.Exception类衍生出来的；    
runtime exception是从java.lang.RuntimeException 或java.lang.Error类衍生出来的，RuntimeException又继承自Exception；    
checked exception用来指示一种调用方能够直接处理的异常情况；    

**Runtime Exception**  
在定义方法时，不需要声明会抛出runtime exception，在调用方法是不需要捕获runtime exception；        
定义方法是必须声明所有可能会抛出的checked exception，在调用方法时，必须捕获它的checked exception,或者throws出去；    
runtime exception指示一种调用方法无法处理货恢复的程序错误；    

#反射 动态代理
[看看java的反射效率](http://yajie.iteye.com/blog/1186522 "看看java的反射效率")


-----------
#Java配置文件

-----------

# JVM虚拟机
## 深入理解Java虚拟机笔记
* [走近Java](http://www.itzhai.com/jvm-note-introduce-java.html "走近Java")
* [自动内存管理机制（Java内存区域与内存溢出异常）](http://www.itzhai.com/jvm-note-automatic-memory-management-mechanism.html "自动内存管理机制（Java内存区域与内存溢出异常）")    
    [Java堆溢出](https://github.com/arthinking/java-code/blob/master/src/me/arthinking/memoryleaktest/HeapOOM.java "Java堆溢出")  
    [more](https://github.com/arthinking/java-code/tree/master/src/me/arthinking/memoryleaktest "more")  
    新生代Minor GC的例子：[点我](https://github.com/arthinking/java-code/blob/master/src/me/arthinking/code4jvmnote/C_3_5_Minor_GC.java "点我")
> 
  1. 分配allocation4时，Eden已经被占用6MB，不够了（总共8MB），所以发生了一次Minor GC；  
  2. GC期间发现已有的3X2MB大小对象无法全部放入Survivor空间,所以只好通过[分配担保机制](http://www.itzhai.com/jvm-note-automatic-memory-management-mechanism-2-html.html#6.5、空间分配担保 "分配担保机制")提前转移到老年代去了；  
  3. GC结束后，4MB的allocation4对象被分配到Eden中，Survivor空闲，老年代占用6MB    

* [自动内存管理机制（垃圾收集器与内存分配策略）](http://www.itzhai.com/jvm-note-automatic-memory-management-mechanism-2-html.html "自动内存管理机制（垃圾收集器与内存分配策略）")    
* [自动内存管理机制（虚拟机性能监控与故障处理工具）](http://www.itzhai.com/jvm-note-automatic-memory-management-mechanism-3.html "自动内存管理机制（虚拟机性能监控与故障处理工具）")     

下面是Java中的内存泄露分析的例子：[Java中内存泄露的分析](http://www.itzhai.com/java-memory-leak-analyze.html "Java中内存泄露的分析") 


* [自动内存管理机制（调优案例分析与实战）](http://www.itzhai.com/jvm-note-automatic-memory-management-mechanism-4.html "自动内存管理机制（调优案例分析与实战）") 
* [虚拟机执行子系统（类文件结构）](http://www.itzhai.com/the-class-file-structure.html "虚拟机执行子系统（类文件结构）")     

对于同步指令monitorenter和monitorexit，编译器会自动产生一个异常处理器，这个异常处理器声明可以处理所有的异常，它的目的就是用来执行monitorexit指令。

* [虚拟机执行子系统（虚拟机类加载机制）](http://www.itzhai.com/the-class-file-structure.html "虚拟机执行子系统（虚拟机类加载机制）")     

加载 -> (验证 -> 准备 -> `解析`) -> 初始化 -> 使用 -> 卸载
其中解析阶段可以在初始化阶段之后再开始，以便支持运行时绑定（动态绑定或晚期绑定）。

JVM中严格规定有且只有一下五中情况必须对类进行“初始化”：[detail](http:// "")(加载、验证、准备自然需要在此之前开始)    

这5种场景称为对一个类的主动引用，其余的称为被动引用，不会触发初始化，以下是几个例子：

> 通过子类引用父类的静态字段，不会导致子类初始化
>     
> 通过数组定义来引用类，不会触发此类的初始化    
> 
> 调用一个类中的常量，不会触发常量的类的初始化    

###JVM内存模型与GC回收机制

###JVM调优和常用工具

**如何dump出当前线程的状态**    
使用[jstack pid](http://www.blogjava.net/jzone/articles/303979.html "jstack pid")进行分析，[Thread Dumps的分析](http://segmentfault.com/blog/yexiaobai/1190000000615690 "Thread Dumps的分析")    

***线程的状态：***    
`Runnable`：    
`Wait oncondition`：    
`Waiting for monitor entry`：    
`in Object.wait()`：        

***案例***：`死锁` & `热锁`    
Java5中加强了堆思索的监测线程Dump中科院直接报告出Java级别的思索。

   
> 如果在多线程的程序中，大量使用 synchronized，或者不适当的使用了它，会造成大量线程在临界区的入口等待，造成系统的性能大幅下降。如果在线程 DUMP中发现了这个情况，应该审查源码，改进程序。(引用)

    
**JVM调优相关经验**


##ClassLoader结构，是否可以自定义一个String类
说说ClassLoader的双亲代理机制

##修改类加载器实现热部署
[ASM修改字节码文件](http://www.ibm.com/developerworks/cn/java/j-lo-hotdeploy/index.html "ASM修改字节码文件")的流程是一个职责链模式，可以通过ASM修改字节码文件的方式，让每次实例化的时候都是用最新版本的Class文件生成实例，从而达到热部署，但是PermGen space还是不可避免。另外，这种方法仅仅是让新实例化的对象是用新的逻辑；    
[Tomcat](http://my.oschina.net/heroShane/blog/198492 "Tomcat")中的热部署实现。


--------------------

#框架源码相关

##并发框架
多线程并发用过哪些

##Spring

##OSGi框架

###如何在一个bundle中加载另一个bundle的Class对象

--------------------
--------------------

#Java Web
##Servlet与Filter的作用，原理，配置

##Session共享机制









