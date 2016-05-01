有这样一个场景：使用`getMulti`一次性读取一个系列的所有手机100个key，请求了100万次，系统最初只有一个MC服务器，随着访问量的增加，负载加大了，于是增加了几个MC服务器，但结果负载反而更加大了。

原因是开始那100个key在一台服务器上获取，现在分不到了几MC服务器，需要访问的服务器增多了，而关键性的因素是我们用到的MC客户端memcached-client，其中的AscIIClient如下：    

```Java
public Map<String, Object> getMulti(String[] keys, Integer[] hashCodes, boolean asString)
  {
    if ((keys == null) || (keys.length == 0)) {
      if (log.isErrorEnabled())
        log.error("missing keys for getMulti()");
      return null;
    }

    Map cmdMap = new HashMap();
    String[] cleanKeys = new String[keys.length];
    for (int i = 0; i < keys.length; i++) {
      String key = keys[i];
      if (key == null) {
        if (log.isErrorEnabled())
          log.error("null key, so skipping");
      }
      else
      {
        Integer hash = null;
        if ((hashCodes != null) && (hashCodes.length > i)) {
          hash = hashCodes[i];
        }
        cleanKeys[i] = key;
        try {
          cleanKeys[i] = sanitizeKey(key);
        }
        catch (UnsupportedEncodingException e) {
          if (this.errorHandler != null)
            this.errorHandler.handleErrorOnGet(this, e, key);
          if (log.isErrorEnabled())
            log.error("failed to sanitize your key!", e);
          continue;
        }

        SchoonerSockIO sock = this.pool.getSock(cleanKeys[i], hash);

        if (sock == null) {
          if (this.errorHandler != null) {
            this.errorHandler.handleErrorOnGet(this, new IOException("no socket to server available"), key);
          }
        }
        else
        {
          if (!cmdMap.containsKey(sock.getHost())) {
            cmdMap.put(sock.getHost(), new StringBuilder("get"));
          }
          ((StringBuilder)cmdMap.get(sock.getHost())).append(new StringBuilder().append(" ").append(cleanKeys[i]).toString());

          sock.close();
        }
      }
    }
    if (log.isDebugEnabled()) {
      log.debug(new StringBuilder().append("multi get socket count : ").append(cmdMap.size()).toString());
    }

    Map ret = new HashMap(keys.length);

    new NIOLoader(this).doMulti(asString, cmdMap, keys, ret);

    for (int i = 0; i < keys.length; i++)
    {
      if ((!keys[i].equals(cleanKeys[i])) && (ret.containsKey(cleanKeys[i]))) {
        ret.put(keys[i], ret.get(cleanKeys[i]));
        ret.remove(cleanKeys[i]);
      }

    }

    if (log.isDebugEnabled())
      log.debug(new StringBuilder().append("++++ memcache: got back ").append(ret.size()).append(" results").toString());
    return ret;
  }   
```

请求多台服务器是串行的，结果导致客户端操作时间累加，请求堆积，最终导致性能下降。

## 解决方法有两个：

一是把串行请求改为并行请求，可以参考spymemcached的并行实现：    
* 第一步，将本次操作构造成一个针对每个 node 的 Operation 对象，加入连接对象中；    
* 第二步，在连接对象中，将所有的 node 操作放入 addedQueue 队列，然后触发 Selector 方式异步非阻塞的执行；

一是把key根据一个系列的手机散列不同的MC服务器上，这样就达到请求一台服务器获取所有的内容了，不过根据就不同的业务场景散列方法也不同，比较不好处理。

或者不使用getMulti这个方法了

必须使用getMulti方法的时候可以把缓存数据复制到另一个memcache集群上，一个集群负责读取一半的keys，但是又会引发需要更多的CPU的问题。

[旁观者的博客](http://www.cnblogs.com/zhengyun_ustc/p/multigethole.html "旁观者的博客")也分析了这类分析，很透彻，提供给大家参考下


该博文发表于：http://www.itzhai.com/mc-use-getmulti-problem.html




