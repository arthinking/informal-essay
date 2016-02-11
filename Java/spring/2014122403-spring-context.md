Context 在 Spring 的 org.springframework.context 包下，前面已经讲解了 Context 组件在 Spring 中的作用，他实际上就是给 Spring 提供一个运行时的环境，用以保存各个对象的状态。下面看一下这个环境是如何构建的。

ApplicationContext 是 Context 的顶级父类，他除了能标识一个应用环境的基本信息外，他还继承了五个接口，这五个接口主要是扩展了 Context 的功能。下面是 Context 的类结构图：

**Context相关的类结构图**

![](https://raw.githubusercontent.com/arthinking/informal-essay/master/images/2014/12/20141224-spring005.png)    

从上图中可以看出 ApplicationContext 继承了 BeanFactory，这也说明了 Spring 容器中运行的主体对象是 Bean，另外 ApplicationContext 继承了 ResourceLoader 接口，使得 ApplicationContext 可以访问到任何外部资源，这将在 Core 中详细说明。

###ApplicationContext 的子类主要包含两个方面：
* ConfigurableApplicationContext 表示该 Context 是可修改的，也就是在构建 Context 中用户可以动态添加或修改已有的配置信息，它下面又有多个子类，其中最经常使用的是可更新的 Context，即 AbstractRefreshableApplicationContext 类。
* WebApplicationContext 顾名思义，就是为 web 准备的 Context 他可以直接访问到 ServletContext，通常情况下，这个接口使用的少。
再往下分就是按照构建 Context 的文件类型，接着就是访问 Context 的方式。这样一级一级构成了完整的 Context 等级层次。

###总体来说 ApplicationContext 必须要完成以下几件事：    
* 标识一个应用环境
* 利用 BeanFactory 创建 Bean 对象
* 保存对象关系表
* 能够捕获各种事件
* Context 作为 Spring 的 Ioc 容器，基本上整合了 Spring 的大部分功能，或者说是大部分功能的基础。


> [Spring 框架的设计理念与设计模式分析](http://www.ibm.com/developerworks/cn/java/j-lo-spring-principle/ "Spring 框架的设计理念与设计模式分析") 
