## 集合类结构

* Collection->Queue,List,Set

* Queue->Deque->ArrayDeque：

  ![image-20230811222334802](/Users/king-mac/Library/Application Support/typora-user-images/image-20230811222334802.png)

* List->ArrayList,LinkedList,Vector

* Set->SortedSet->TreeSet

  ![image-20230811225233750](/Users/king-mac/Library/Application Support/typora-user-images/image-20230811225233750.png)

* Set->HashSet->LinkedHashSet

* Map->HashMap->LinkedHashMap

* Map->SortedMap->TreeMap

  ![image-20230811225200905](/Users/king-mac/Library/Application Support/typora-user-images/image-20230811225200905.png)

## List，Map，Set

* 父类 Collection

* List，Set 单列集合；Map 双列集合

* List 有序可重复；Map ，Set 无序不可重复

* set 位置由 hashcode 决定，但元素顺序是无序的

* List：LinkedList 链表改快查慢，ArrayList 数组改慢查快，Vector 线程安全

* Map：HashMap 键无序；LinkedHashMap 键有序；SortedMap -> TreeMap 键排序; HashTable 不存 null kv，线程安全

* Set：HashSet 底层 HashMap 实现，LinkedHashSet 有序基于 LinkedHashMap 实现

  ![image-20230811230327920](/Users/king-mac/Library/Application Support/typora-user-images/image-20230811230327920.png)

## HashMap 排序

* list = new ArrayList<>(map.entrySet());
* Collections.sort(list,()->(return a-b;)) 排序
* 遍历 list 放入 linkedHashMap

## 线程安全集合

* 同步集合 Coolections.synchronizedList/Map/Set
* Jdk1.5 后并发集合
* java.util.concurrent 包下 ConcurrentHashMap
* 使用锁分段技术比同步集合效率更高,java8使用自旋锁