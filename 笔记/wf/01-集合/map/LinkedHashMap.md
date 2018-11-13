# LinkedHashMap
*LinkedHashMap继承HashMap,实现了Map接口，是哈希表和链接列表（**双向链表**）实现。具有可预知的迭代顺序（**插入顺序和访问顺序，默认为插入顺序**）。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。*

[详解LinkedHashMap如何保证元素迭代的顺序](http://www.php.cn/java-article-362041.html)

[图解LinkedHashMap原理](https://www.jianshu.com/p/8f4f58b4b8ab)

## LinkedHashMap与HashMap的区别
**LinkedHashMap也是一个HashMap,但是内部维持了一个双向链表,可以保持顺序**