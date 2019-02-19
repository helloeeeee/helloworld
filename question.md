# 基础篇
## 基本功
* **面向对象的特征**：抽象、封装、多态（面向对象编程的三大特性）、继承
* **什么是多态**：多态表现为一个变量在运行时可以指向其他多种类型
> 多态性是对象多种表现形式的体现。
* **int 和 Integer 有什么区别**:
	1. int是基本数据类型，Integer是int的封装类型，Integer有方法。
	2. 泛型参数中只能使用Integer而不能使用int。
	3. Integer和int进行运算时，Integer会先拆箱成为int再进行运算(基本型和基本型封装型进行“==”运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较)。
	4. 两个Interger值进行比较，如果值在-128~127之间，返回true，因为Interger.valueOf()方法回去这个区间的对象进行缓存。
* **重载和重写的区别**：
	1. 重载要求方法的参数列表不一样，重写要求签名和返回值完全一致。
	2. 重写的类的访问权限必须大于等于父类访问权限。
	2. 重写发生在继承类中，重载发生在同一个类中。
* **final, finally, finalize 的区别**：
	1. final用于修饰类，变量和方法。修饰类表示此类不能被继承，修饰变量表示变量不可变，修饰方法表示方法不能被重写。static final表示编译器常量。
	2. finally用于try-catch语句中，无论是否catch到异常finally块必然被执行，主要用执行一些资源关闭等操作。
	3. finalize类似于c#中的析构函数，主要用于在垃圾收集器将该对象清除之前执行一些操作。如果一个对象被判定需要执行finalize方法，那么他会被加入到一个叫做F-QUEUE的队列中，等待执行finalize方法。finalize方法是对象逃脱死亡的最后一次机会。因为finalize方法只会执行一次，该对象下一次回收，将不会执行finalize方法。finalize操作代价高昂，不建议使用。
* **说说反射的用途及实现**：
	1. 反射用于在运行时构造一个类，获取类属性、方法，实例化类，调用方法等。
	2. 反射主要用于框架中，如spring
* **自定义注解的场景及实现**“
	1. 自定义注解主要用于进行权限控制、日志记录等。
	2. 实现步骤：
		1. 创建注解类，并标记类、方法等。
		2. 使用spring AOP等进行切面拦截，利用反射获取注解值，并进行相应操作。
* **HTTP 请求的 GET 与 POST 方式的区别**：
	1. get方法的参数会显示在浏览器地址栏中，并且有长度限制，而post方法则不会
	2. get方法主要用于查询接口，而post则用于增删改操作
	3. post操作比get操作更安全，因为敏感信息不能Get方式传递

* **session 与 cookie 区别**：
	1. cookie存储在客户端，而session存储在服务端
	2. **cookie可以被禁用，但是session不能**
	3. 服务端可以在header头中添加set-cookie来添加cookie

* **session 分布式处理**：
	1. 粘性session：当用户访问集群中的一个服务器后，后续访问强制其中到该服务器上。当集群中一个机器宕机后，session会丢失，会造成单点故障。
	2. session复制：将session广播到其他机器上。广播会耗时，存在延时问题。
	3. session集中管理，将session存储在缓存系统中，每次访问都从缓存中获取session。实现较复杂，依赖缓存的稳定。
	4. 通过spring进行spring session共享。

* **JDBC 流程**：
	1. 加载驱动程序：`class.forName()`
	2. 创建Connection对象，`DriverManager.getConntction(url,username,password)`
	3. 创建Statement（PreparedStatement,CallableStatement)对象
	4. 执行sql语句
	5. 关闭资源

* **MVC 设计思想**：
	1. M（model),V(view），C（controller)
	2. 视图层发送请求，分发到controller中，controller执行请求后输出到视图层。

* **equals 与 == 的区别**：
	1. ==用于比较两个对象是否是同一个对象（即内存地址是否相同）
	2. equals是Object的方法，默认使用==比较。通过重写equals方法，可以自定义比较方法,如String中比较的是String的值。

## 集合
* **List和Set的区别**:
	1. List有序，可以存储相同的值。Set无序，不能存储相同的值，**重复元素会被覆盖**
	2. 都是接口，并继承Collection接口
	3. **list因为有序，可以使用下标访问元素，set无序，所有只能使用迭代器，不能使用下标访问**。

* **List和Map的区别**:
	* list是对象集合，允许重复，且有序
	* map是键值对集合，不允许key重复。

* **Arraylist 与 LinkedList 区别**：
	1. ArrayList基于数组实现，LinkedList基于链表实现。他们都线程不安全，ArrayList查询速度较LinkedList快，插入删除速度较慢。
	2. ArrayList实现RandomAccess接口，该接口为标记接口，表示支持随机访问，因此遍历ArrayList使用for循环比使用迭代器的速度要更快。
	3. LinkedList继承SequentialList类，只支持按次序访问
	4. 他们内部都维护了一个ModCount变量和expectedModCount，当进行结构性变化时，modCount会加一，在执行方法时，如果发生了ModCount！=expectModCount时，说明又多个线程对该变量进行了修改，则抛出并发异常。
	5. ArrayList在添加元素时会调用方法确保内容足够，如果不够，将调用grow()方法扩容，每次扩容将容量增加为原来的1.5倍（oldCapacity+(oldCapacity>>1)），扩容需要调用Array.CopyOf复制元数组，及其耗时，因此，初始化一个合适的ArrayList大小是一个良好实践。

* **ArrayList 与 Vector 区别**：
	1. 都继承AbstractList类，Vector是Stack类的父类。
	2. **扩容方式不同，ArrayList为1.5倍，vector可以设置扩容容量，如果设置了则容量为原容量+扩容容量，否则为原容量的两倍。ArrayList不能设置扩容容量**
	3. Vector是线程安全的,使用过的是Synchronized，因此效率极低，不建议使用（可以使用CopyOnWrite等线程安全容器)

* **HashMap和HashTable的区别**：
	1. HashMap和HashTable都是键值对集合，HashMap线程不安全，HashTable线程安全。
	2. HashMap允许key为null，HashTable不允许。
	3. HashTable虽然使用synchronized实现线程安全，因此效率很低，如果为了线程安全问题使用HashTable，建议使用ConcurrentHashMap。**可以使用`Collections.synchronizedMap`使HashMap变为线程安全，原理是利用Synchronized**
	4. **HashMap的迭代器是Iterator,它fail-fast的，而HashTable使用的是枚举器emumeratror,不是fail-fast的。
	5. **hash值不同，hashtable中直接使用了对象的hashcode，而hashmap使用的是重新计算的hash值**

* **HashSet和HashMap的区别**
	1. HashSet是对象集合，实现Set接口，继承AbstractSet。HashMap是键值对集合，实现Map接口，继承AbstractMap。
	2. HashSet内部使用HashMap存储，这也是为什么HashSet不能存储相同元素,因为HashMap的key不允许重复，重复的话会被覆盖。

* **HashMap和ConcurrentHashMap的区别**：
	1. HashMap线程不安全，ConcurrentHashMap线程安全。
	2. ConCurrentHash不允许空的键值，但是HashMap允许。
	
* **HashMap的工作原理**：
	* 1.7版本
		1. HashMap大方向上是个Entry数组，每个Entry都是个链表。
		2. put操作时先根据HashMap的Hash算法获得hash值得到数组下标，如果该位置为空，则直接插入，否则在该位置上以链表形式存放，新加入的放在链表头。如果新加入的key值在该bucket中已经存在，那么将覆盖。
		3. get操作使用Hash值找到bucket，遍历链表根据key值查找value值。
		4. 当HashMap中的元素个数超过数组大小*loadFactor(负载因子，默认值0.75),HashMap将会发生扩容（Resize）。扩容操作会重新计算hash表中的所有元素，因此性能消耗极大，使用头插法重新插入节点，这也是HashMap线程不安全的一个重要体现。
		5. HashMap扩容容量必须是2的幂，这是因为hashmap获取index的hash算法为`hash&(length-1)`,length-1的二进制很定位全是1，此时hash值和length-1进行按位与之后得到的就是%后的余数，即该算法相当于取余运算，之所以不使用%操作符，是因为计算机里位运算是基本运算，效率远高于取余运算符。
	* 1.8版本
		1. java8中hashmap利用了红黑树，当链表的长度超过8之后，会将链表准换为红黑树。因为红黑树的查找的时间复杂度要优于链表（O(logN),O(N))
		2. java8中使用Node来替换java7中的Entry,基本没啥区别，不过Entry只能用于链表的情况，红黑树需要使用TreeNode
	
* **ConcurrentHashmap的工作原理**
	* 1.7版本
		1. concurrentHashMap是一个Segment（分段锁）数组，segment继承了ReentrantLock，每次加锁操作锁住的也是segment，因此每个segment是线程安全的，对于同一个segment的访问是互斥的,对于不同segment访问则是可以并发进行的，ConcurrentHashMap正式通过segment实现部分线程安全。
		2. concurrentHashMap有一个concurrencyLevel参数，该参数可以理解为并发数，默认值为16，即concurrentHashMap有16个Segment,所以理论上可以同时支持16个线程并发写
		3. segment数组不能扩容，扩容的segment数组中的某个HashEntry，扩容后，容量为原来的两倍。
	* 1.8版本
		1. 同样引入了红黑树，但不在使用segment来保证线程安全，取而代之的是使用了CAS+Synchronized
	
## 线程
* **创建线程的方式及实现**：
	1. 继承Thead类，实现run()方法，
	```
	 class MyThread extend Thread{
		public void run(){
			//...
		}
	}
	```
	```
	public static void main(String[] args){
		MyThread myThread=new MyThread();
		myThread.run();
	}
	```


	2. 实现Runable接口,通过Thread.start()方法来启动线程
	```
	class MyRunnable implement Runnable{
		public void run(){
			//...
		}
	}
	```
	```
	public static void main(String[] args){
		Thread thread=new Thread(new MyRunnable());
		thread.start();
	}
	```
	
   
	3. 实现Callable接口,callable接口与runnable相比，他可以有返回值，返回值通过FutureTask封装,通过Thread.start()方法来启动线程
	```
	class myCallable implement Callable<String>{
		public String call(){
			return "mycallable";
		}
	}
	```
	```
	public static void main(String[] args){
		MyCallable myCallable=new MyCallable();
		FutureTask<Integer> futureTask=new FutureTask(myCallable);
		Thread thread=new Thread(futureTask);
		thread.start();
		Sysout.out.ptintln(futureTask.get());
	}
	```

	4. 优先使用实现接口的方法，因为java不支持多重继承,但是可以实现多个接口。并且，继承整个类开销过大，可能类知要求方法能够执行。
	
* **sleep()、join()、yield()有什么区别**
	1. sleep()是线程类方法，调用该方法，线程将从运行态转为显示等待。sleep不会释放锁。调用sleep()方法需要捕获异常（InterruptedException）。
	2. 在线程中调用join()方法会挂起当前线程，直到目标线程结束。
	3. yield()方法只是对线程调度器的一种建议，表示当前线程已经完成了生命周期中的重要部分，可以切换给其他线程来执行。

* **AQS理解**
	1. AQS(Abstract queue synchronizer，抽象队列同步器)是java.util.concurrent包的核心。包括常用的ReentrantLock、CountDownLatch、Semaphore等。
	2. AQS没有锁的概念，它内部维护一个叫做state的votaile变量，在不同场合有不同的含义（可以认识是锁）。state为0时表示锁没有被占用。
	3. AQS围绕State提供两种基本操作"获取"和"释放"，有条双向队列(从源码来看，AQS类中有个内部类Node,这个Node有prev和next指针，并且标记了是独占还是共享的，看起来是个链表）存放阻塞的等待线程，并提供一系列判断和处理方法

* **countDownLatch原理**
	1. countdownLatch中有个内部类Sync继承AQS,在Countdownlatch初始化时，会设置aqs的state值。
	2. 调用countdownlatch的await()方法会直接调用AQS的acquireShareInterruptibly()方法，该方法会尝试获取共享锁，如果state！=0,则获取失败，将当前线程加入阻塞队列。
	3. 当调用countdown()方法时，每调用一次countdown()实际就是释放锁的操作，每调用一次，计数值减少1，释放锁的操作使用CAS,当计数器为0，代表所有子线程都执行完了，那么将执行doReleaseShared唤醒被await阻塞的线程。

* ** CyclicBarrier 原理**
	1. cyclicBarrier实际上使用的是ReentrantLock的condition，构造方法中parties表示需要到达的线程数量，count表示等待到达barrier的数量，barrierCommand表示开启barrier后需要执行的操作。
	2. 当调用CyclicBarrier的await方法后，会调用dowait()，该方法将会获取ReentrantLock并锁定，然后将等待的线程数量减一，如果count=0，说明partirs个线程到达barrier,执行预订的Runable方法后，更新换代，准备下一次使用。

* **Semaphore 原理**
	1. semaphore类似Countdownlatch使用AQS操作。也分为公平和非公平两种。构造函数中permits是许可数量，将设置为AQS的state值。
	
* **CountDownLatch与CyclicBarrier的区别**
	1. CyclicBarrier的计数器通过reset()方法可以循环使用，而CountDownLatch不行。
	2. CountDownLatch直接使用了AQS,而CyclicBarrier使用的是ReentrantLock。
	3. countDownLactch的目标方向更多是一个线程等待多个线程，而CyclicBarrier则是互相等待。

* **ThreadLocal 原理**
	1. 在Thread类中有一个成员变量ThreadLocals,该变量的类型是ThreadLocal类中的静态内部类ThreadLocalMap，它的键是threadLocal，值为就是变量的副本。通过ThreadLocal的get()方法可以获取该线程变量的本地副本，在get方法之前要先set,否则就要重写initialValue()方法。

* **线程池的实现原理**
	1. 线程池工构造函数中的几个重要参数
		1. corePoolSize:表示核心池的大小
		2. maximumPoolSize:线程池最大线程数
		3. KeepAliveTime:表示线程没有任务执行时最多保持多久会终止。
		4. TimeUnit:keepAliveTime的时间单位
		5. WorkQueue:阻塞队列，主要选择有ArrayBlockingQueue,LinkedBlockingQueue和SynchronousQueue
		6. threadFactory:线程工厂，用来创建线程
		7. handler:拒绝策略，当线程池中线程数量达到最大线程数，就会执行拒绝策略。主要有AbortPolicy、DiscardPolicy、DiscardOldestPolicy、CallerRunsPolicy。
	2. 线程池是由Worker类负责完成任务，Worker继承了AQS。
	3. 任务提交给线程池之后的处理策略
		1. 如果当前线程池中的线程数目小于corePoolSize，则每来一个任务，就会创建一个线程去执行这个任务；
		2. 如果当前线程池中的线程数目>=corePoolSize，则每来一个任务，会尝试将其添加到任务缓存队列当中，若添加成功，则该任务会等待空闲线程将其取出去执行；若添加失败（一般来说是任务缓存队列已满），则会尝试创建新的线程去执行这个任务；
		3. 如果当前线程池中的线程数目达到maximumPoolSize，则会采取任务拒绝策略进行处理；
		4. 如果线程池中的线程数量大于 corePoolSize时，如果某线程空闲时间超过keepAliveTime，线程将被终止，直至线程池中的线程数目不大于corePoolSize；如果允许为核心池中的线程设置存活时间，那么核心池中的线程空闲时间超过keepAliveTime，线程也会被终止。


* **创建线程池的几种方式和使用场景**
	1. Executor是最顶层的接口，他只有一个execute方法用来执行Runnable任务。ExecutorService接口扩展了Executor，提供了对线程生命周期的管理方法,听shutdown，submit等。AbstractExecutorService实现了ExecutorService,提供了例如submit等一些方法的默认实现。ThreadPoolExecutor则继承了AbstractExecutorService。
	2. ThreadPoolExecutor预设了一些已经定制好的线程池，可以通过Executors里的工厂方法来创建
		1. newFixedThreadPool:corePoolSize和MaximumPoolSize设置为传入的固定值，线程池创建后，线程数量将会固定不变，适合需要线程很稳定的场合。
		2. newSingleThreadExecutor:newSingleThreadPool是线程数量恒定为1的newFixedThreadPool,可以保证池内线程串行。
		3. newCachedThreadThreadPool:生成一个会缓存的线程池，如果有空闲线程则会复用线程，如果没有空闲线程，会新建线程，新线程的空闲时间超过一分钟则会被回收。
		4. newScheduledThreadPool:创建一个可定时执行任务的线程池。
	
* **线程的生命周期**
	1. new
	2. runnable
	3. blocking
	4. time waiting
	5. waiting
	6. terminated


## 锁机制
* **ReentrantLock的实现原理**
	1. reentrantLock的内部类Sync继承了AQS,分为公平锁FairSync和非公平锁NonfairSync。
	2. lock操作先使用CAS判断state为0来表示锁是否被占用，如果没有被占用则独占锁，否则尝试获取锁（acquire()),如果获取成功，独占锁，重入则+1，如果获取失败，创建一个Node节点加入则阻塞队列
	3. unlock操作先尝试释放锁，并判断队列下个节点是否需要唤醒，然后决定唤醒被挂起的线程。
	
* **volatile实现原理**
	1. volatile有两个功能：保证可见性和禁止指令重排序
	2. volatile在各个线程的工作内存中不存在一致性问题的原因是因为每次使用前都要刷新，执行引擎看不到不一致性的情况因此可以认为不存在一致性问题。
	3. volatile之所以能够禁止指令重排序是因为volatile会在本地代码中插入许多内存屏障来保证处理器不乱序执行。

* **synchronized实现原理**
	1. 对象在内存中的布局分为对象头、实例数据和填充。其中对象头分为运行时数据（mark word）、类型指针和数组长度，其中mark word中存储了hash值、分代年龄、锁标志、锁对象指针等。
	2. 重量级锁（synchronized）的锁标识位为10，指针指向monitor对象的起始地址。每个锁对象都有一个monitor对象与之关联，当一个monitor对象被线程持有，这个线程就处于锁定状态。monitor对象是由ObjectMonitor实现的，它是c++代码。
	3. ObjectMonitor对象有两个队列_waitSet和_EntityList，用来保存ObjectWaiter对象列表（每个等待锁的线程都会被封装成为ObjectWaiter),_owner指向持有ObjectMonitor的线程，当多个线程访问一段同步代码时，首先会进入_EntityList等待，当获取对象的monitor之后进入_owner区，并把monitor的owner变量设置当前线程并且将计数器count+1，若调用waiter方法，将释放当前持有的monitor，并进入_waitSet区域等待被唤醒。同时owner对象设置null，计数器count-1.
	4. 同步语句块使用的指令是moniterenter和moniterexit
	5. 同步方法使用的是ACC_SYNCHRONIZED标识

* **Synchronized和ReentrantLock的区别**
	1. synchronized是jvm实现的，ReentrantLock是JDK实现。
	2. synchronized经过新版本的优化，例如自旋锁等，现在两个性能想当。
	3. ReentrantLock支持公平锁和非公平锁，Synchronized只支持非公平锁。
	4. ReentrantLock的锁等待状态可以放弃等待（thread.interrupt）
	5. ReentrantLock可以绑定多个条件(condition)
	6. 除非使用ReentrantLock的高级功能，否则建议使用Synchronized,因此ReentrantLock是JVM实现的，原生支持，而ReentrantLock可能不是所有JDK都支持。并且Synchronized不会出现没有释放锁而导致的死锁问题。

* **CAS乐观锁**
	1. CAS(compare and swap)操作需要三个操作数，分别是内存位置，旧的预期值和新值。当内存位置值符合旧的预期值时就用新值去更新旧值，否则不执行操作，但是无论是否更新了内存位置的值，都会返回旧值。
	2. CAS只有JDK1.5之后才能使用。
	3. CAS存在ABA问题，JUC为了解决这个问题，提供了一个原子类AtomicStampedReference，它可以控制变量的版本来保证ABA问题不会出现。不过并没什么用，解决ABA问题，改为传统的互斥同步可能比提供的这个类更搞笑。

* **什么是乐观锁**
	1. 乐观锁假设线程冲突的几率很小，直接去操作数据，如果发现数据(通过版本号等)已被更改，则放弃操作，并重复所需操作
	2. 如果线程几率大，那么使用乐观锁会重复很多次操作（包括查询数据库等），那么会很低小，此时应该考虑使用悲观锁。

# 框架篇		
## spring
* **BeanFactory 和 ApplicationContext 有什么区别**
	1. BeanFactory和ApplicationContext都是Spring容器，BeanFactory会延时加载bean，而Application会在容器启动的时候，一次性创建所有bean，因此如果bean创建错误，启动时就会报错。
	2. ApplicationContext继承了BeanFacotry,并提供了更为高级的功能，包括
		* 利用MessageSource实现国际化功能
		* 资源访问
		* 强大的事件传播，利用观察者模式，事件类继承ApplicationEvent类，监听类继承ApplicationListener接口，发布类继承ApplicationContextAware接口，调用ApplicationContext.publicshEvent发布事件。
		* 载入多个有继承关系的上下文
	3. 开发者直接使用ApplicationContext

* **spring bean的声明周期**
	1. spring容器读取bean并实例化
	2. 对bean进行属性注入
	3. 如果bean实现了BeanNameAware方法，则调用setBeanName()传递bean name。
	4. 如果bean实现了BeanFactoryAware方法，则调用setBeanFactory()方法传入工厂。
	5. 如果有BeanPostProcessors和bean关联，则调用beanpostBeforeInitialization()方法。
	6. 如果bean实现了InitializingBean,则调用afterProperties方法。
	7. 如果bean定制了init-method，则调用
	8. 如果有beanPostProcessors和bean关联，则调用beanPostAftetInitializtion()方法。
	9. 如果bean实现了DisposableBean,执行DiSposebaleBean的destory()
	10. 如果bean定制了destory-method，则调用destoryMethod方法。

* **spring IOC是如何实现的**
	1. IOC:通过IOC容器完成对象的创建、管理、依赖注入等。
	2. 将bean载入IOC容器：创建beanFactory时，调用loadBeanDefinition()方法，大致过程为：通过ResourceLoader完成资源文件定位，容器再通过BeanDefinitionReader来完成定义信息的解析和Bean信息的注册，解析得到BeanDefinition之后就放入IOC容器中。实际上BeanDefinition存储在DefaultListableBeanFactory的一个ConcurrentHashMap中。

* **Spring AOP**
	1. 