Bean 组件在 Spring 的 org.springframework.beans 包下。这个包下的所有类主要解决了三件事：Bean 的定义、Bean 的创建以及对 Bean 的解析。对 Spring 的使用者来说唯一需要关心的就是 Bean 的创建，其他两个由 Spring 在内部帮你完成了，对你来说是透明的。

Spring Bean 的创建是典型的工厂模式，他的顶级接口是 BeanFactory，下图是这个工厂的继承层次关系：    

**Bean工厂的继承关系**
![](https://raw.githubusercontent.com/arthinking/informal-essay/master/images/2014/12/20141224-spring002.png)

BeanFactory 有三个子类：ListableBeanFactory、HierarchicalBeanFactory 和 AutowireCapableBeanFactory。但是从上图中我们可以发现最终的默认实现类是 DefaultListableBeanFactory，他实现了所有的接口。那为何要定义这么多层次的接口呢？查阅这些接口的源码和说明发现，每个接口都有他使用的场合，它主要是为了区分在 Spring 内部在操作过程中对象的传递和转化过程中，对对象的数据访问所做的限制。例如 ListableBeanFactory 接口表示这些 Bean 是可列表的，而 HierarchicalBeanFactory 表示的是这些 Bean 是有继承关系的，也就是每个 Bean 有可能有父 Bean。AutowireCapableBeanFactory 接口定义 Bean 的自动装配规则。这四个接口共同定义了 Bean 的集合、Bean 之间的关系、以及 Bean 行为。    

**Bean定义的类层次关系图**    

Bean 的定义主要由 BeanDefinition 描述，如下图说明了这些类的层次关系：
![](https://raw.githubusercontent.com/arthinking/informal-essay/master/images/2014/12/20141224-spring003.png)
Bean 的定义就是完整的描述了在 Spring 的配置文件中你定义的 <bean/> 节点中所有的信息，包括各种子节点。当 Spring 成功解析你定义的一个 <bean/> 节点后，在 Spring 的内部他就被转化成 BeanDefinition 对象。以后所有的操作都是对这个对象完成的。

Bean 的解析过程非常复杂，功能被分的很细，因为这里需要被扩展的地方很多，必须保证有足够的灵活性，以应对可能的变化。Bean 的解析主要就是对 Spring 配置文件的解析。这个解析过程主要通过下图中的类完成：    

**Bean的解析类**
    
![](https://raw.githubusercontent.com/arthinking/informal-essay/master/images/2014/12/20141224-spring004.png)

[通过XmlBeanFactory实现启动Spring IoC容器](https://github.com/arthinking/java-code/blob/master/src/main/java/me/arthinking/spring/ioc/BeanFactoryTest.java "通过XmlBeanFactory实现启动Spring IoC容器")

> [Spring 框架的设计理念与设计模式分析](http://www.ibm.com/developerworks/cn/java/j-lo-spring-principle/ "Spring 框架的设计理念与设计模式分析") 
