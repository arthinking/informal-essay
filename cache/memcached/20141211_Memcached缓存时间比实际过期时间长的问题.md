查看[Memcached服务器端源码](https://github.com/memcached/memcached/blob/master/memcached.c "Memcached服务器端源码")，发现MC服务器端判断缓存的有效时间是按照如下方法的：    
##计算服务器启动后多少秒该key会失效
###1、如果是一个超过30天的时间，则认为是一个Unix时间戳：    
失效时间 = 设置的过期时间- 设置的过期时间距离memcached服务器启动时间的秒数;

###2、如果是一个30天内的有效时间，则认为是一个时间长度：    
失效时间 = 设置的过期时间长度 + current_time

> 具体可以参考[这里](http://my.oschina.net/u/1255608/blog/163342 "这里")     

如果MC服务器启动之后，对系统时间进行了调整，那么第一种情况就会得到不准确的有效时间了

[相关bug](http://www.couchbase.com/issues/browse/MB-11548 "相关bug")

