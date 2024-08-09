## 集合类结构

* Collection->Queue,List,Set
* Queue->Deque->ArrayDeque

* List->ArrayList,LinkedList,Vector
* Set->HashSet->LinkedHashSet
* Set->SortedSet->TreeSet（有序Set集合）

* Map->HashMap->LinkedHashMap
* Map->SortedMap->TreeMap（有序Map集合）


## List，Map，Set

* 父类 Collection

* List，Set 单列集合；Map 双列集合；

* List 有序可重复；Map ，Set 无序不可重复

* Set 元素顺序是无序的，在数组中的位置由元素的 hashcode 决定，数组存储键值对值为固定值，有 hash 冲突存到链表或红黑树里

* List：LinkedList 链表改快查慢，ArrayList 数组改慢查快，Vector 线程安全

* Map：HashMap 键无序，在数组中的位置由元素的 hashcode 决定，数组存储键值对，有 hash 冲突存到链表或红黑树里；LinkedHashMap 键有序（底层双向链表记录顺序）；SortedMap -> TreeMap 键排序（底层红黑树保证顺序）; HashTable 不存 null 键值，线程安全；

* Set：HashSet 底层 HashMap 实现，LinkedHashSet 有序基于 LinkedHashMap 实现


## HashMap 排序

* list = new ArrayList<>(map.entrySet());
* Collections.sort(list,()->(return a-b;)) 排序
* 遍历 list 放入 linkedHashMap

## 线程安全集合

* 同步集合 Coolections.synchronizedList/Map/Set
* Jdk1.5 后并发集合，java.util.concurrent 包下 ConcurrentHashMap，使用锁分段技术比同步集合效率更高,java8使用自旋锁