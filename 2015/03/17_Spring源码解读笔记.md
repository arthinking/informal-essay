#Spring-core
##org/springframework/core/io/相关内容
###UrlResource.getInputStream()  #166
```
ResourceUtils.useCachesIfNecessary(con);
```
设置`URLConnection`的`UseCaches`为false，主要是为了防止在Windows下jar包被锁住。

###AbstractFileResolvingResource 
里面包含了JBoss vfs协议URL文件的获取。通过静态内部类 VfsResourceDelegate 进行处理的

###ResourceLoader
是一个策略接口，其 getResource(String location)方法根据传入不同的location，返回不同的Resource。其实现类是一个可以单独使用的ResourceLoader实现类，ResourceEditor也使用了这个类

###DefaultResourceLoader
ResourceLoader的默认实现，可以单独使用，ResourceEditor使用了该类，是AbstractApplicationContext的基类。

##说说Spring容器的加载过程
XmlBeanFactory通过Resource装载Spring配置信息并启动IoC容器；
然后通过BeanFactory#getBean(beanName)方法从IoC容器获取Bean；
通过BeanFactory启动IoC容器时，并不会初始化配置文件中定义的Bean，初始化动作发生在第一个调用时；
对于单实例的Bean来说，BeanFactory会缓存Bean实例，所以第二次使用getBean()获取Bean的时候将直接从IoC容器的缓存中获取Bean实例。

##Spring MVC一次执行的原理

##列举出5种常用的SQL语句优化方式

##说说两种数据库引擎的区别



##猿题库

问题
描述
回答
参考网址
举报test


