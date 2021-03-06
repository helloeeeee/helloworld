# MySQL

### 一、存储引擎
*数据库管理系统使用数据引擎进行数据的创建、更新、查询、删除。不同的存储引擎提供不同的存储机制，索引技巧，锁定水平等。**简单来说，存储引擎就是指表的类型以及表在计算机上的存储方式。** MySQL的核心就是存储引擎。四种常用存储引擎：**InnoDB、MyISAM、MEMORY、Archive。***

**在MySQL中，不需要在整改服务器中石油同一种存储引擎，针对具体要求，可以对每个表使用不同的存储引擎。**

#### （1）、InnoDB存储引擎
*InnoDB是事务型数据库的首选，**它支持事务安全表、支持行锁定、外键。** 是MySQL默认的存储引擎。*

##### 物理文件结构：
*共享表空间：共享表空间存储方式使用.ibdata文件，所有表共同使用一个ibdata文件*
*独享表空间：每个表都有一个.frm文件和.ibd文件。.frm文件存放表元数据，.ibd文件存放表数据和索引数据。*

* InnoDB的表数据与索引数据是存放在.ibd或者.ibdata文件。之所以用两种文件来存放innodb的数据，是因为innodb的数据存储方式能够通过配置来决定是使用**共享表空间**存放存储数据，还是用**独享表空间**存放存储数据。
* InnoDB有支持事务及安全的日志文件

##### 事务
InnoDB给MYSQL提供了具有提交、回滚和崩溃恢复能力的事务安全存储引擎。

##### 锁机制的改进
实现了行级锁，为承受高并发增加了竞争力。

##### 外键
InnoDB支持外键完整性约束。

##### 支持索引类型
树锁。

##### 增删改查性能
如果执行大量的增删改操作，推荐使用InnoDB存储引擎，它在删除操作时是对行删除，不会重建表。

##### COUNT(*)问题
InnoDB存储引擎会遍历表以计算数量，效率较低。

#### （2）、MyISAM存储引擎
*MyISAM拥有较高的插入、查询速度，但不支持事物。*

##### 物理文件结构：
* 表相关的元数据（表的结构定义信息等）都存放在.frm文件。
* 表的数据存储在.myd文件。
* 表的索引相关信息存储在.myi文件。

##### 支持的索引类型
* BTree（最常见）
* R-Tree索引
* Full-Text索引

##### 锁机制
支持表级锁定

##### 事务处理
不支持
##### 增删改查性能
SELECT性能较高，适合执行查询较多的情况使用
##### COUNT(*)问题
MyISAM存储引擎记录表行数，所以在使用COUNT(*)时，只需取出存储的行数，而不用遍历表，效率较高

#### （3）、MyISAM与InnoDB的区别
[MyISAM与InnoDB的区别](https://blog.csdn.net/lc0817/article/details/52757194)

* InnoDB支持事物，而MyISAM不支持事物
* InnoDB支持外键，而MyISAM不支持
* InnoDB支持行级锁，而MyISAM支持表级锁
* InnoDB支持MVCC, 而MyISAM不支持
* InnoDB不支持全文索引，而MyISAM支持
* InnoDB不能通过直接拷贝表文件的方法拷贝表到另外一台机器， myisam 支持

#### （4）、MEMORY存储引擎
*MEMORY存储引擎将表中的数据存储到内存中，为查询和引用其他表数据提供快速访问。*

##### 支持的索引类型
HASH索引、BTree索引

##### 其他特征
* MEMORY不支持BLOB或TEXT列
* 可以在一个MEMORY表中有非唯一键值
* MEMORY表在所由客户端之间共享
* 当不再需要MEMORY表的内容时，要释放被MEMORY表使用的内存，应该执行DELETE FROM或TRUNCATE TABLE，或者删除整个表（使用DROP TABLE）

### 二、索引
*索引是对数据库中一列或者多列的值进行排序的一种结构，索引用于快速查找在某一列中有特定值的行。不使用索引，MySQL必须从第一条记录开始读完整个表，直到找出相关的行。表越大，查询数据所花费的时间越多，如果表中查询的列有一个索引，MySQL能快速到达一个位置去搜索数据文件，而不必查看所有数据。*

#### （1）、索引的优缺点
**优点：**
* 唯一索引可以保证数据库表中每一行数据的唯一性。
* 可以大大加快数据查询速度。
* 在实现数据的参考完整性方面，可以加速表和表之间的连接。
* 在使用分组和排序子句进行数据查询时，也可以显著减少查询中分组和排序的时间。

**缺点：**
* 创建索引和维护索引耗费时间，随着数据量的增大，耗费时间也会增多。
* 索引需要占用磁盘空间。除了数据表占数据空间外，每一个索引也要占一定的物理空间。
* 对表中数据进行新增、修改、删除操作时，索引页需要进行维护。降低了数据维护速度。

#### （2）、索引的分类
**普通索引和唯一索引**
* 普通索引：是MySQL中的基本索引，允许索引重复和空值。
* 唯一索引：索引值必须唯一，但允许空值。

**单列索引和组合索引**
* 单列索引：一个索引只能保护一列，一个表可以有多个单列索引。
* 组合索引：表的多个字段组合为一个索引。**查询时，符合最左原则，索引才被使用。**

*最左原则：当创建联合索引 key test_col1_col2_col3 on test(col1,col2,col3)。实际上建立了(col1)、(col1,col2)、(col1,col2,col3)三个索引。*
[深入浅析Mysql联合索引最左匹配原则](https://www.jb51.net/article/142840.htm)

**全文索引**
全文索引类型为FULLTEXT，在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值。全文索引可以在CHAR、VARCHAR或者TEXT类型的列上创建，MySQL中只有MyISAM支持全文索引。

#### （3）、索引的原则
*索引设计不合理或者缺少索引都会对数据库和应用程序的性能造成障碍。*

* 索引并非越多越好，一张表如果有大量的索引，不仅占用磁盘空间。而且会影响INSERT、UPDATE、DELETE性能，因为当表中数据发送变化时，索引也会进行相应的调整和更新。
* 避免对频繁更新的表设计过多的索引，并且索引中的列尽可能少，对于经常查询的字段创建索引，避免不必要的字段添加索引。
* 对于数据量少的表最好不要使用索引。因为查询花费的时间可能比遍历索引时间短。
* 在条件表达式中经常用到的不同值较多的列上创建索引，在不同值较少的列上不创建。比如性别字段只有男女，就没有必要创建索引。如果创建索引，不但不会提高查询效率，还会降低更新效率。
* 当唯一性值某种数据本身的特征时，指定唯一索引。使用唯一索引能确保定义的列的数据完整性，提高查询效率。
* 在频繁排序或者分组（group by 和 order by）的列上创建索引。如果排序的列有多个，可以创建联合索引。

#### （4）、聚集索引与非聚集索引
* 聚集索引：聚簇索引的数据的物理存放顺序与索引顺序是一致的（索引中的数据物理存放地址和索引的顺序是一致的）。
* 非聚集索引：索引的逻辑顺序与磁盘上的物理存储顺序不同。
* 聚集索引查询效率快，只要找到第一个索引值记录，其余就连续性的记录在物理也一样连续存放。聚集索引对应的缺点就是修改慢，因为为了保证表中记录的物理和索引顺序一致，在记录插入的时候，会对数据页重新排序。
* 非聚集索引：查询效率慢，因为会出现二次查询。但是解决二次查询，查询

聚集索引的定义：
* 如果一个主键被定义了，那么这个主键就是作为聚集索引。
* 如果没有主键被定义，那么该表的第一个唯一非空索引被作为聚集索引。
* 如果没有主键也没有合适的唯一索引，那么innodb内部会生成一个隐藏的主键作为聚集索引，这个隐藏的主键是一个6个字节的列，改列的值会随着数据的插入自增。

### 三、MySQL的事务
[MySQL的事务与锁](https://www.cnblogs.com/xzwblog/p/6956817.html)

*InnoDB存储引擎支持事务。*

**1、默认情况下，MySQL每执行一条语句，都是一个单独的事务。**

**2、在MySQL的命令行模式下，事务都是自动提交的。即执行SQL语句后就会马上COMMIT。**

#### （1）、事务的4个特性(ACID)
*Atomicity（原子性）、Consistency（一致性）、Isolation（隔离性）、Durability（持久性）*
* 原子性：事务是一个不可分割的工作单位，事务中的操作要么全部成功，要么全部失败。
* 一致性：一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。
* 隔离性：隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。即要达到这么一种效果：对于任意两个并发的事务T1和T2，在事务T1看来，T2要么在T1开始之前就已经结束，要么在T1结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行。
*  持久性：持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

#### （2）、事务的作用
*保证了用户的每一次操作都是可靠的，即便出现了异常的访问情况，也不至于破坏后台数据的完整性。*

#### （3）、并发下事务会产生的问题
* 更新丢失：当多个事务选择一行数据进行更新，由于每个事务不知道其他事务的存在就会发生丢失更新问题，即最后的更新覆盖之前的更新。**需要更新的数据加必要的锁来解决。**
* 脏读：一个事务正在对一条记录做修改操作，在事务完成并提交前，这条记录的数据就处于不一致状态。这时，另一个事务也来读取同一条数据，如果不加控制，第二个事务就会读取“脏”数据，出现脏读。
* 不可重复读：一个事务读取一条数据后，在结束读取之前，另一个事务完成了对数据的修改。当第一个事务再次读取时，就会出现两次读取数据不同。
* 幻读：一个事务按相同的查询条件重新读取以前检索过的数据，却发现其他事务插入了满足其查询条件的新数据，这种现象就称为“幻读”。

**脏读、不可重复读、幻读都是数据库读一致性问题，必须由数据库提供一定的事务隔离机制来解决。**

#### （4）、4种事务隔离级别
**事务隔离级别是并发控制的整体解决方案，其实际上是综合利用各种类型的锁和行版本控制（MVCC）来解决并发问题。**
* READ_UNCOMMITTED（读未提交）：能够读取到没有被提交的数据，所以很明显这个级别的隔离机制无法解决脏读、不可重复读、幻读中的任何一种，因此很少使用。
* READ_COMMITED（读已提交）：事务中只能看到已提交的修改，能够防止脏读，但是无法限制不可重复读和幻读。**（未加锁）**
* REPEATABLE_READ（重复读）：读取了一条数据，对这条数据加当前锁，这个事务不结束，当前锁不释放，别的事务就不可以改这条记录。但是幻读的问题还是无法解决。可以通过NEXT-KEY锁（间隙锁）来解决幻读问题。**（加锁）**
* SERLALIZABLE（串行化）：最高的事务隔离级别，不管多少事务，挨个运行完一个事务的所有子事务之后才可以执行另外一个事务里面的所有子事务，这样就解决了脏读、不可重复读和幻读的问题了。**从MVCC并发控制退化为基于锁的并发控制。不区别快照读与当前读，所有的读操作均为当前读，读加读锁 (S锁)，写加写锁 (X锁)**。

### 四、MySQL的锁
[MySQL 加锁处理分析](http://hedengcheng.com/?p=771#_Toc374698320)

#### MySQL的锁机制
*根据MySQL存储引擎的不同，MySQL有三种锁机制，即：表锁、行锁、页锁。分别对于存储引擎：MyISAM、InnoDB、BDB。*

* 表锁：开销大，加锁快；不会出现死锁；锁的粒度大，发生锁冲突的概率最高，并发度也最低。
* 行锁：开销小，加锁蛮；会出现死锁；锁的粒度小，发生锁冲突的概率最低。并发度也最高；
* 页锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

#### （1）、InnoDB行锁
**InnoDB行锁是通过给索引上的索引项加锁来实现的。即：只有通过索引条件检索数据，InnoDB才会使用行锁，否则使用表锁。**

*InnoDB中的行锁分为 **共享锁、排他锁**。（MyISAM 分别叫做读锁和写锁）*

[mysql共享锁与排他锁](https://www.cnblogs.com/boblogsbo/p/5602122.html)

* 共享锁：若一个事务T对数据A加共享锁，事务T只能读取A不能修改A，且其他事务只能对A加共享锁，不能加排他锁。这保证了其他事务可以读A，但在T释放A上的共享锁锁之前不能对A做任何修改。
* 排他锁：若一个事务T对数据A行加了排他锁，事务T可以读A也可以修改A，且其他事务就不能对该行的加其他锁，包括共享锁和排他锁，获取排他锁的事务可以对数据就行读取和修改。这保证了其他事务在T释放A上的排他锁之前不能再读取和修改A。

#### （2）、MVCC（并发控制协议）
*与MVCC相对的，是基于锁的并发控制。MVCC最大的好处：读不加锁，读写不冲突，极大的增加了系统的并发性能。*

**在MVCC中，读操作可以分为快照读、当前读。**
* 快照读：读取的是记录的可见版本，不用加锁。
* 当前读：读取的是记录的最新版本，当前读返回的记录，都会加上锁，保证其他事务不会再并发修改这条记录。
* 快照读：简单的select操作，属于快照读，不加锁。如：select * from table where ?;
* 当前读：特殊的读操作，插入/更新/删除操作，属于当前读，需要加锁。如：1、select * from table where ? lock in share mode; 2、select * from table where ? for update; 3、insert 4、 update 5、delete

**一个Update操作的具体流程**
![一个Update操作的具体流程](http://pic.yupoo.com/hedengcheng/DnJ6RwcV/medish.jpg "一个Update操作的具体流程")

* MySQL Server根据where条件，采用当前读，查询一条记录，然后InnoDB会将记录返回并加锁。
* 待MySQL Server收到这条加锁的记录之后，会再发起一个Update请求，更新这条记录。

#### （3）、死锁
*死锁是指两个或者多个事务在同一资源上相互作用，并请求锁定对方占用的资源，从而导致恶性循环的现象。当多个事务试图以不同的顺序锁定资源时，就可能产生死锁。多个事务同时锁定同一个资源时，也会产生死锁。*

##### 第一种情况死锁
![死锁情况：两个事务的两条SQL](http://pic.yupoo.com/hedengcheng/DnJ6RTaA/medish.jpg "死锁情况：两个事务的两条SQL")
*当第一个事务执行Select时，会对主键id为1的一行数据加X锁。此时，第二个事务执行Delete操作，会对主键id为5的一行数据加X锁。由于执行第一个事务的Update操作，需要在主键id为5的行数据加X锁，而此时第二个事务已经在id为5的行数据加了X锁，导致了锁冲突，阻塞。并且第二个事务的Delete操作需要在主键为1的行数据加X锁，而id为1的数据已经加了X锁，导致了锁冲突，阻塞。而因为两个事务都没提交，且需要在对方资源加锁，导致死锁。*

##### 第二种情况死锁
![死锁情况：两个事务的一条SQL](https://images2015.cnblogs.com/blog/590093/201608/590093-20160829132000527-940926950.jpg "死锁情况：两个事务的一条SQL")

*针对Session 1，从name索引出发，读到的[hdc, 1]，[hdc, 6]均满足条件，不仅会加name索引上的记录X锁，而且会加聚簇索引上的记录X锁，加锁顺序为先[1,hdc,100]，后[6,hdc,10]。而Session 2，从pubtime索引出发，[10,6],[100,1]均满足过滤条件，同样也会加聚簇索引上的记录X锁，加锁顺序为[6,hdc,10]，后[1,hdc,100]。发现没有，跟Session 1的加锁顺序正好相反，如果两个Session恰好都持有了第一把锁，请求加第二把锁，死锁就发生了。*


### 五、数据库常见优化方法
[数据库常见优化方法](https://www.cnblogs.com/luokakale/p/7242839.html)
#### （1）、选取最适合的字段属性
*随着数据量的存取，**数据库中的表越小，那么查询越快。** 在创建表单的时候，应该将表中字段设置的尽量小。*

比如：在定义邮政编码这个字段时，如果将其设置为CHAR(255),显然给数据库增加了不必要的空间，甚至使用VARCHAR这种类型也是多余的，因为CHAR(6)就可以很好的完成任务了。对于整型字段，尽量使用MEDIUMINT而不是BIGIN。

#### （2）、使用连接（JOIN）来代替子查询(Sub-Queries)

#### （3）、使用联合(UNION)来代替手动创建的临时表

#### （4）、使用索引
