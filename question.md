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

	