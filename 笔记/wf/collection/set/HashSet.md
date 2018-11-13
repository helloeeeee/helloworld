# HashSet
* HashSet实现了Set接口，基于哈希表(实际上是HashMap)，它不保证set 的迭代顺序；特别是它不保证该顺序恒久不变。此类允许使用null元素。
* HashSet是基于HashMap实现的，相关HashSet的操作，基本上都是直接调用底层HashMap的相关方法来完成
* 对于HashSet中保存的对象，请注意正确重写其equals和hashCode方法，以保证放入的对象的唯一性。