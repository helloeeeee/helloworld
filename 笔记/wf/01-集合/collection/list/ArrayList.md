# ArrayList
* ArrayList是List接口的可变数组（**顺序表**）的实现。
* 顺序表的存储地址是连续的，成块的，查询的时候直接顺序遍历内存。所以查询速度快
* 在ArrayList中增加或者删除某个元素，通常会调用System.arraycopy方法，这是一种极为消耗资源的操作。
* 由于插入或者删除的时候，需要移动元素（**插入数据向后移动，删除数据向前移动**），所以速度慢，效率低。
* 每个ArrayList实例都有一个用来存储列表元素的数组(**默认长度为10**),该数组长度大于等于列表长度（**至少等于**）。
* ArrayList是非同步的。
* 允许元素为null。

### 调整数组容量
*向ArrayList中存储元素时（向数组中添加元素），都会去检查添加后元素的个数是否会超过当前数组的长度，如果超出，**数组将会进行扩容**。数组进行扩容时，会将老数组中的元素重新拷贝一份到新数组中，每次数组容量的增长大约是器容量的1.5倍。*
 
### Fail-Fast机制
同HashMap中的Fail-Fast机制
    
### 常见5大面试题
#### 一、ArrayList的大小是如何自动增加的？
    略，见调整数组容量
#### 二、什么情况下你会使用ArrayList？什么时候你会选择LinkedList？
    多数情况下，当你遇到访问元素比插入或者是删除元素更加频繁的时候，你应该使用ArrayList。另外一方面，当你在某个特别的索引中，插入或者是删除元素更加频繁，或者你压根就不需要访问元素的时候，你会选择LinkedList。
#### 三、如何复制某个ArrayList到另一个ArrayList中去？写出你的代码？
* 使用clone方法，ArrayList newArray = oldArray.clone();（**浅拷贝**）
* 使用ArrayList构造方法，比如：ArrayList myObject = new ArrayList(myTempObject);（**浅拷贝**）
* 使用Collection的copy方法。

#### 四、当传递ArrayList到某个方法中，或者某个方法返回ArrayList，什么时候要考虑安全隐患？如何修复安全违规这个问题呢？
    当array被当做参数传递到某个方法中，如果array在没有被复制的情况下直接被分配给了成员变量，那么就可能发生这种情况，即当原始的数组被调用的方法改变的时候，传递到这个方法中的数组也会改变。

#### 五、在索引中ArrayList的增加或者删除某个对象的运行过程？效率很低吗？解释一下为什么？
    在ArrayList中增加或者是删除元素，要调用System.arraycopy这种效率很低的操作

### 解决ArrayList的线程不安全
* 使用Collections.synchronizedList()。
* 使用synchronized关键字。
    
### ArrayList与Vector的区别
* ArrayList不是线程安全的，Vector是线程安全的(**Vector实现线程安全的方式是在修改数据时锁住整个Vector，即方法级别的synchronized。效率低**)
* 当Vector或ArrayList中的元素超过它的初始大小时,**Vector会将它的容量翻倍,而ArrayList只增加50%的大小**，这样,ArrayList就有利于节约内存空间。

### ArrayList与LinkedList的区别
* ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构
* 对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。
* 对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。