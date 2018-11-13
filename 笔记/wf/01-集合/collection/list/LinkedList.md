# LinkedList
* LinkedList是List接口的双向链表的实现。
* 不需要扩容。
* 增删操作只需要移动指针，所以效率高，时间快。
* 查询操作时间效率低，虽然底层在根据index查询Node时，会根据index判断目标在Node的前半段还是后半段，然后决定是顺序还是逆序查询，以提升时间效率。
* LinkedList是非同步的。
* 允许元素为null。

### Fail-Fast机制
同HashMap中的Fail-Fast机制

### 总结
* LinkedList的批量增加，是靠for循环遍历原数组，依次执行插入节点操作。对比ArrayList是通过System.arraycopy完成批量增加的。增加一定会修改modCount。
* 通过下标获取某个node 的时候，会根据index处于前半段还是后半段 进行一个折半，以提升查询效率
* 删也一定会修改modCount。按下标删，也是先根据index找到Node，然后去链表上unlink掉这个Node。 按元素删，会先去遍历链表寻找是否有该Node，如果有，去链表上unlink掉这个Node。
* 改也是先根据index找到Node，然后替换值。改不修改modCount。
* 查本身就是根据index找到Node。
* 所以它的CRUD操作里，都涉及到根据index去找到Node的操作。

[面试必备：LinkedList源码解析（JDK8）](https://blog.csdn.net/zxt0601/article/details/77341098)