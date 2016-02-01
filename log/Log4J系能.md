#性能问题

关于Log4j的性能问题，可以先阅读以下官方文档的一点说明：[Performance](http://logging.apache.org/log4j/1.2/manual.html "Performance")

[Log4J 2](http://www.reader8.cn/jiaocheng/20130810/2235056.html "Log4J 2")

在[Apache Log4j 2.0值得升级吗](http://www.infoq.com/cn/news/2014/08/apache-log4j2 "Apache Log4j 2.0值得升级吗")这篇文章中阐述了Log4j 2.0的高性能。而[Log4j 2.0](http://www.infoq.com/cn/news/2014/07/apache-log4j2.0-publish "Log4j 2.0")的API是和Log4j 1.x系列的API不兼容的

在一个项目中如果需要在不同的模块设置不同的日志级别，并且输出到不同的日志文件中，可以这样设置不同的logger：
> http://www.theserverside.com/news/thread.tss?thread_id=31659

