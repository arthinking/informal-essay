#安装JDK
> [ubuntu 12.04 LTS 安装配置JDK](http://jingyan.baidu.com/article/b0b63dbfd5db8b4a48307027.html "ubuntu 12.04 LTS 安装配置JDK")    

下载jdk-xxx.bin
    
解压文件    
chmod u+x jdk-xxx.bin    
./jdk-xxx.bin    

复制解压出jdk-xxx文件夹到/usr/lib下    
sudo mkdir -p /usr/lib/jvm/    
cp -r jdk-xxx /usr/lib/jvm/jdkxxx

#配置JDK
> [ubuntu 12.04安装jdk](http://blog.chinaunix.net/uid-26404477-id-3471246.html "ubuntu 12.04安装jdk")

    
```shell
apt-get install gedit
sudo gedit /etc/profile
```

or    
```shell
cd /etc/
vim profile
```

```shell
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_10
export JRE_HOME=/usr/lib/jvm/jdk1.7.0_10/jre 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH

export PATH=$JAVA_HOME/bin:$PATH
```
关闭，保存，使用source更新一下profile文件
```shell
source ~/.profile
```

如果之前系统里面已经安装了openjdk，可以使用如下方法将默认jdk更改过来：

将系统默认的jdk修改过来
```shell
$ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_10/bin/java 300
```
输入sun jdk前的数字就好了    

```shell
$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_10/bin/javac 300
$ sudo update-alternatives --config java 
$ sudo update-alternatives --config javac
```

#使用VisualVM监控远程主机JVM
> [visualvm监控jvm及远程jvm监控方法](http://www.blogjava.net/titanaly/archive/2012/03/20/372318.html "visualvm监控jvm及远程jvm监控方法")

##直接使用JDK
如果是直接监控JVM，则在 `/etc/profile` 文件里面添加如下配置：
```xml
export JAVA_OPTS="-Dcom.sun.management.jmxremote.port=<port> -Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.authenticate=false -Djava.rmi.server.hostname=<hostname>  
-Dcom.sun.management.jmxremote.ssl=false"
```    

详细参考：
> [使用Java自带的VisualVM监控远程主机JVM内存使用情况](http://www.cnblogs.com/chenying99/archive/2012/06/21/2557208.html "使用Java自带的VisualVM监控远程主机JVM内存使用情况")

##使用resin
在resin.conf配置文件中添加如下配置：
```xml
<jvm-arg>-Xdebug</jvm-arg>
<jvm-arg>-Dcom.sun.management.jmxremote</jvm-arg>
<jvm-arg>-Djava.rmi.server.hostname=192.168.75.34</jvm-arg>
<jvm-arg>-Dcom.sun.management.jmxremote.port=9934</jvm-arg>
<jvm-arg>-Dcom.sun.management.jmxremote.ssl=false</jvm-arg>
<jvm-arg>-Dcom.sun.management.jmxremote.authenticate=false</jvm-arg>
```

在客户端使用JMX连接即可
