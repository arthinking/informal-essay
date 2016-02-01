#1、安装
##1.1、上传resin程序包
```shell
mkdir /home/mmt/soft
#将resin-pro-3.1.12.tar.gz程序上传至soft目录下，解包：
tar zxvf resin-pro-3.1.12.tar.gz
#将解包后的程序copy至/usr/local目录下：
cp -rf resin-pro-3.1.12 /usr/local
#将 resin-pro-3.1.12 改名为 resin
mv /usr/local/resin-pro-3.1.12/ /usr/local/resin
#进入/usr/local/resin/目录
cd /usr/local/resin
```


> 相关资源：    
> 
> [ubuntu 编译并安装resin3.1.12+nginx1.2.6](http://blog.csdn.net/tegwy/article/details/8870873 "ubuntu 编译并安装resin3.1.12+nginx1.2.6")

##1.2、安装
```shell
./configure --with-java-home=/usr/lib/jvm/java/jdk1.6.0_26
#启动resin，看是否安装成功
/usr/local/resin/bin/httpd.sh start
#如果能正常显示页面则表示安装成功，现在可以停止并设置相关配置文件了。
/usr/local/resin/bin/httpd.sh stop
```

#2、配置
##JVM参数设置

在resin/conf/resin.conf中的<jvm-arg>标签中进行配置，这将是启动JVM的初始化参数。

具体的参数机器作用：
```xml
<!-- - JVM 参数设置 -->
<jvm-arg>-Xms512 m</jvm-arg>
<!--jvm 最小内存，也是启动 resin后的默认内存分配值-->
<jvm-arg>-Xmx512 m</jvm-arg> <!-- make ms=mx to reduce GC times -->
<!--jvm最大内存，当内存使用超过 Xms分配的值之后会自动向这个最大值提升，一般配置成最大最小值相等，理论上能够降低 GC 垃圾收集的时间，可按实际进行配置-->
<jvm-arg>-Xmn86m</jvm-arg>
<!--内存分配增量，当内存需求超过 Xms值之后进行第一次分配请求的内存值，一般为 Xmx的 1/3-1/4 ；开始时候可以先屏蔽，当应用出现OutOfMemory的时候再打开也可以-->
<jvm-arg>-XX:MaxNewSize=256m</jvm-arg>
<jvm-arg>-XX:PermSize=128m</jvm-arg>
<jvm-arg>-XX:MaxPermSize=256m</jvm-arg>
<!--以上三项是为了减少 OutOfMemory 而配置的，
是每个java编译执行的时候最多能一次申请 jvm 内存空间的值，
以上默认配置基本够用，但依然出OutOfMemory 的时候可以适当调大，
但不能超越 Xmx的值；
开始时候可以先屏蔽，当应用出现 OutOfMemory的时候再打开也可以-->
<jvm-arg>-Xss256k</jvm-arg> <!-- jvm Stack config -->
<jvm-arg>-Djava.awt.headless=true</jvm-arg> <!--允许使用验证码-->
<jvm-arg>-Djava.net.preferIPv4Stack=true</jvm-arg> <!-- disable IPv6 -->
<jvm-arg>-Doracle.jdbc.V8Compatible=true</jvm-arg><!--针对 oracle10的兼容配置-->
<watchdog-arg>-Dcom.sun.management.jmxremote</watchdog-arg>

<!-- 强制 resin 强制重启时的最小空闲内存 -->
<memory-free-min>2M</memory-free-min>

<!-- 最大线程数量 -->
<thread-max>256</thread-max>

<!-- 套接字等待时间 -->
<socket-timeout>65s</socket-timeout>

<!-- 配置 keepalive -->
<keepalive-max>128</keepalive-max>
<keepalive-timeout>15s</keepalive-timeout>
```   

**日常遇到的OOM和频繁发生GC 情况分析**

* java.lang.OutOfMemoryError: Java heap space
heap 空间不足，可能是 -Xmx 配得过大，或者系统内存不足或泄漏

* java.lang.OutOfMemoryError: PermGen space
持久代内存不足，存在大量系统类被加载或 jpa 等架构频繁使用，需要增加 Perm的内存配置

* java.lang.OutOfMemoryError:unable to create native thread

* 空闲内存不足以建立新的线程，减少 max-threads 的配置，增加空闲内存数量

在实际的生产环境中，以下三个东西经常用到，所以在这里提一下，也算是简单的优化。

```xml
<thread-max>512</thread-max>
<!--最大线程数影响 resin 的系统负载能力以及 java 进程的内存占用-->
<keepalive-max>128</keepalive-max>
<!--keepalive 的最大数量，对网络性能有影响-->
```

###JVM调优相关：    
* [JVM调优](http://blog.csdn.net/tyrone1979/article/details/1274458 "JVM调优")    
* [resin优化经验](http://www.opendigest.org/article.php/450 "resin优化经验")    
* [resin](http://www.caucho.com/resin-3.0/performance/jvm-tuning.xtp#garbage-collection "resin")    
* js/html/css/jpg/gif 等静态文件由 nginx 提供服务，剩下的由nginx以upstream方式代理到后端resin处理，以减少resin 提供这些静态文件访问的性能问题。

Resin 及 jvm 优化，是一项基于提供服务的应用上进行一段相对长时间的测试进行，由于每个项目都有其自身特点，只有根据这些特点来进行优化，才能把该项目配置得更好 ，不可能硬套到其它项目上 。


**测试线程并发量**    

先将resin.conf文件中的thread-min，thread-max，thread-keepalive三个参数设置的比较大，分别写上，1000，3000，1000，当然这是根据你的机器情况和可能同时访问的数量决定的，如果你的网站访问量很大的，应该再适当放大。

然后观察任务管理器中的java线程变化情况，看看到底是线程达到多大的时候，java进程当掉的。我的是在379左右当掉。

然后将thread-min，thread-max，thread-keepalive分别写为150，400，300；也就是将当掉的时候的最大值稍微放大点，作为thread-max的值，因为该系统一般不会超过这个值。然后其他两个参数根据情况设置一下。
