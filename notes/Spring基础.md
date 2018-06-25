* ##### 概述

  ![](/assets/spring-arch.png)

  * 核心容器  
    由spring-core，spring-beans，spring-context，spring-context-support和spring-expression等模块组成

  * 数据访问/集成层  
    包括 JDBC，ORM，OXM，JMS 和事务处理模块

  * Web 层  
    由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成

  * 其他  
    AOP，Aspects，Instrumentation，Web 和测试模块

* ##### Spring核心

  * ###### IOC

    * ###### [依赖注入和自动装配](https://blog.csdn.net/u012843873/article/details/52399206)
  * ###### AOP

    面向方面的编程需要把程序逻辑分解成不同的部分称为所谓的关注点。跨一个应用程序的多个点的功能被称为横切关注点，这些横切关注点在概念上独立于应用程序的业务逻辑。有各种各样的常见的很好的方面的例子，如日志记录、审计、声明式事务、安全性和缓存等。
* ##### Spring MVC

  * [跟踪请求](https://www.cnblogs.com/leskang/p/6101368.html)  
    ![](/assets/spring-mvc-archimport.png)
* ##### 后端中的Spring

  * ###### 数据持久化

    * ###### mybatis
  * ###### 缓存数据

    * ###### redis
* ##### Spring Boot
* ##### [Spring事务](https://blog.csdn.net/bao19901210/article/details/41724355)

  Spring并不直接管理事务，而是提供了多种事务管理器，他们将事务管理的职责委托给Hibernate或者JTA等持久化机制所提供的相关平台框架的事务来实现。 Spring事务管理器的接口是org.springframework.transaction.PlatformTransactionManager，通过这个接口，Spring为各个平台如JDBC、Hibernate等都提供了对应的事务管理器，但是具体的实现就是各个平台自己的事情了。

  * ###### 编程式和声明式事务的区别

     Spring提供了对编程式事务和声明式事务的支持，编程式事务允许用户在代码中精确定义事务的边界，而声明式事务（基于AOP）有助于用户将操作与事务规则进行解耦。  
     简单地说，编程式事务侵入到了业务代码里面，但是提供了更加详细的事务管理；而声明式事务由于基于AOP，所以既能起到事务管理的作用，又可以不影响业务代码的具体实现



