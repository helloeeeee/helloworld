### 一、BeanFactory 和 FactoryBean 有什么区别
* BeanFactory是IOC容器最基本的形式，给具体的IOC容器（如XmlBeanFactory、ApplicationContext）提供了规范。
* FactoryBean为IOC容器中Bean的实现提供了更加灵活的方法。使用简单工厂模式和装饰模式，可以在getObject()方法中灵活配置。解决某些Bean在实例化时，有很多复杂逻辑的问题（如果使用<bean id="" class=""> 则需要很多配置信息。）。

### 二、BeanFactory 和 ApplicationContext 有什么区别
* BeanFactory接口是Spring中比较原始的Factory。原始的BeanFactory无法支持Spring的许多插件，如AOP功能、Web应用等。
* ApplicationContext接口继承BeanFactory，具有BeanFactory所有功能。ApplicationContext还提供了以下功能：1、MessageSource,提供国际化消息访问。2、资源访问，如URL、文件。3、事件传播(观察者模式)。4、载入多个（有继承关系的）上下文，使得每个上下文都专注于一个特定的层次，比如应用的Web层。
* BeanFactory采用的是延迟加载的方式来注入Bean，即只有在使用Bean的时候才进行加载实例化。而ApplicationContext在容器启动的时候，一次性创建了所有的Bean。
* BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor。区别是：BeanFactory需要手动注册，而ApplicationContext是自动注册。

#### ApplicationContext比BeanFactory多的功能
##### （1）、利用MessageSource进行国际化 
BeanFactory没有继承MessageSource，所以不支持国际化功能。

##### （2）、强大的事件传播
基本上设计事件传播都离不开观察者模式。ApplicationContext的事件机制主要是通过ApplicationEvent和ApplicationListener两个接口。**当ApplicationContext发布一个事件时，所以扩展了ApplicationListener的Bean都讲会接受这个事件，并进行相应的处理。**

##### （3）、底层资源的访问 
ApplicationContext扩展了ResourceLoader(资源加载器)接口，从而可以用来加载多个Resource，而BeanFactory是没有扩展ResourceLoader。

##### （4）、载入多个（有继承关系的）上下文
与BeanFactory通常以编程的方式被创建不同的是，ApplicationContext能以声明的方式创建，如使用ContextLoader。当然你也可以使用ApplicationContext的实现之一来以编程的方式创建ApplicationContext实例 。 

### 三、Spring Ioc容器的初始化过程
[Spring之IOC容器初始化过程](https://www.cnblogs.com/myadmin/p/5838795.html)

*1、BeanDifinition的Resource的定位（Bean的定义文件定位）*

*2、BeanDifinition的载入与解析*

*3、BeanDifinition在Ioc容器的注册*

#### BeanDifinition的Resource的定位（Bean的定义文件定位）
*Resource指的是BeanDifinition的资源定位，Resource由ResourceLoader通过统一的Resource接口完成。这个Resource对各种形式的BeanDifinition的使用都提供了统一的接口。比如使用ClassPathApplicationContext时，使用ClassPathResource。使用FileSystemXmlApplicationContext时，使用FileeSystemResource。*

#### BeanDifinition的载入与解析
*这个过程是把用户定义的Bean表示成Ioc容器的数据结构，这个数据结构就是BeanDifinition。BeanDifinition实际上就是Bean对象在IOC容器中的抽象（Bean的信息），通过BeanDifinition，使Ioc容器更方便对Bean进行管理。*

#### BeanDifinition在Ioc容器的注册
*这个操作是通过调用BeanDifinitionRegistry接口实现的。这个过程是把载入与解析的BeanDifinition向Ioc容器进行注册，即注入到ConcurrentMap中，键是BeanName，值为BeanDifinition。*

### 四、Spring Bean的加载过程
[Spring Bean的加载过程](http://geeekr.com/read-spring-source-1-how-to-load-bean/)

* web项目通过web.xml中的ContextLoaderListener初始化IOC容器
* ContextLoaderListener继承了ContextLoader,实现了ServletContextListener接口。当server容器启动时，收到事件初始化。
* ContextLoader的initWebApplicationContext()方法。首先判断servletContext是否已经注册了WebApplicationContext（通过servletContext的getAttribute()查询是否注册）。如果有则抛出异常，避免重复注册。
* 如果没有注册调用ContextLoader的createWebApplicationContext()方法。查询需要加载的XmlWebApplicationContext类，在org.springframework.web.context.WebApplicationContext同一个包下面的ContextLoader.properties文件读取默认设置，反射XmlWebApplicationContext。
* 在configureAndRefreshWebApplicationContext()方法里实例化XmlWebApplicationContext。
* 将ServletContext设置成XmlWebApplicationContext属性，方便获取ServletContext。
* 读取web.xml中的contextConfigLocation参数，读取applicationContext.xml文件，并将值设置进XmlWebApplicationContext中。
* 最后核心方法wac.refresh()。这个方法会完成资源加载、配置文件解析、BeanDefinition的注册、组件的初始化等核心工作。


### 五、Spring Bean的生命周期
![Spring Bean的生命周期](https://images2015.cnblogs.com/blog/801753/201702/801753-20170204111521089-1301937796.png "Spring Bean的生命周期")

[Spring Bean的生命周期](https://www.cnblogs.com/kenshinobiy/p/4652008.html)

*实例化前，1、先检查本地的单例缓存中是否已经加载过了。2、没有加载再检查earlySingleton缓存是否已经加载过Bean。3、没有加载，进行基本的检查和检查操作，包括bean是否是pototype（pototype会抛出异常）、是否是抽象的、将beanName加入的alreadyCreated这个Set中等。*

* Spring容器 从XML 文件中读取bean的定义，并实例化bean。
* Spring根据bean的定义填充所有的属性。
* 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
* 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
* 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
* 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
* 如果有定制的init-mothod初始化方法（配置文件中<bean id="" class="" init-mothod=""/>），则调用。
* 如果有BeanPostProcessors和bean关联，这些bean的postProcessAfterInitialization() 方法将被调用。
* 如果bean实现了DisposableBean，注册需要执行销毁方法的Bean。

### 六、Spring Aop原理

### 七、动态代理（cglib 与 JDK）

### 八、