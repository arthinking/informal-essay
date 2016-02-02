#1、配置
##1.1、安装
```
sudo apt-get install memcached
```
##1.2、启动
> Memcached的基本设置：
>     
> -p 监听的端口    
> -l 连接的IP地址, 默认是本机    
> -d start 启动memcached服务    
> -d restart 重起memcached服务    
> -d stop|shutdown 关闭正在运行的memcached服务    
> -d install 安装memcached服务    
> -d uninstall 卸载memcached服务    
> -u 以的身份运行 (仅在以root运行的时候有效)    
> -m 最大内存使用，单位MB。默认64MB    
> -M 内存耗尽时返回错误，而不是删除项    
> -c 最大同时连接数，默认是1024    
> -f 块大小增长因子，默认是1.25    
> -n 最小分配空间，key+value+flags默认是48    
> -h 显示帮助    
> mixi的设置,单台:    

```shell
# 每台mc服务器仅启动一个mc进程，分配1G内存
/usr/bin/memcached -p 11211 -u nobody -m 1000 -c 512

# 启动mc
/usr/share/memcached/scripts/start-memcached
```    

注意：32位的操作系统中，每个进程最多只能够使用**2G**的内存，所以需要更大的内存的时候就只能进行集群了。（用同一台服务器进行集群，TCP连接数就会成倍增加，x86_64的操作系统可以分配超过2G的内存）；

mc进程的实际内存分配量要比置顶的内存要大一些，所以如果置顶分配的内存太大了，有可能导致内存交换（swap）。

#2、集群配置
通过magent能够让缓存写入到多个不同的memcached里面

##2.1、安装使用magent
###2.1.1、编译安装libevent
```shell
wget http://monkey.org/~provos/libevent-1.4.9-stable.tar.gz
tar zxvf libevent-1.4.9-stable.tar.gz
cd libevent-1.4.9-stable/
./configure --prefix=/usr
make && make install
cd ../
```

###2.1.2、编译安装Memcached：
```shell
wget http://danga.com/memcached/dist/memcached-1.2.6.tar.gz
tar zxvf memcached-1.2.6.tar.gz
cd memcached-1.2.6/
./configure --with-libevent=/usr
make && make install
cd ../
```

###2.1.3、编译安装magent：
```shell
mkdir magent
cd magent/
wget http://memagent.googlecode.com/files/magent-0.5.tar.gz
tar zxvf magent-0.5.tar.gz
/sbin/ldconfig
sed -i "s#LIBS = -levent#LIBS = -levent -lm#g" Makefile
make
cp magent /usr/bin/magent
cd ../
```

###2.1.4、集群配置
集群两台服务器，实现缓存备份。
高可用网络架构：    
![](https://raw.githubusercontent.com/arthinking/my-document/master/images/2014/12/20141217-mc001.png)

**启动两个mc进程，端口分别为11211,11212**
```shell
memcached -m 1 -u root -d -l 127.0.0.1 -p 11211
memcached -m 1 -u root -d -l 127.0.0.1 -p 11212
```

**启动两个magent进程，端口分别为10000,10001**
```shell
magent -u root -n 51200 -l 127.0.0.1 -p 10000 -s 127.0.0.1:11211 -b 127.0.0.1:11212
magent -u root -n 51200 -l 127.0.0.1 -p 10001 -s 127.0.0.1:11212 -b 127.0.0.1 11211
```

-s为要写入的memcached，-b为备份用的memcached    

#3、使用
##3.1、清空缓存
```
telnet 127.0.0.1 11211
flush_all
quit
```

#4、监控
##4.1、stats
```shell
telnet 127.0.0.1 11211
stats
```

> 相关资源：
>     
> [memcached+magent实现memcached集群](http://www.cnblogs.com/happyday56/p/3461113.html "memcached+magent实现memcached集群")     
>
> [memcache集群服务：memagent配置使用  ](http://zhumeng8337797.blog.163.com/blog/static/10076891420113431424757/ "memcache集群服务：memagent配置使用  ")

> 发表于：http://www.itzhai.com/mc-config-and-monitoring.html
