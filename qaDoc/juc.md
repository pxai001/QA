## CAS

   ```java
   // volatile 修饰变量，满足可见性每次使用变量时都会同步主内存值
   // java.util.concurrent.atomic，比较并交换，用线程内存值和主内存值
   AtomicBoolean.compareAndSet(false, true); 
   // 比较，如果相同则执行，否则自旋锁更新线程内存值直至相同，原子执行 ++ --，前者返回更新前后者返回更新后
   AtomicInteger.getAndIncrement,incrementAndGet,getAndDecrement,decrementAndGet   
   ```

## LOCK

```java
// java.util.concurrent.lock
Lock lock = new ReentrantLock(true);  默认 false 非公平锁；true 公平锁
// java.util.concurrent.condition
Condition condition = lock.newCondition();
condition.await();
condition.signal();
// java.util.concurrent.semaphore
Semaphore semaphore = new Semaphore(3, true);
semaphore.acquire();
semaphore.release();
```

## 阻塞队列

### BlockingQueue 方法
  * 插入：add (e失败抛出异常)，offer(e失败返回特殊值)，put(e失败阻塞)，offer(obj, int timeunit, status)
  * 移除：remove(失败抛出异常)，poll(失败返回特殊值)，take(失败阻塞)，，超时退出poll(time,unit)
  * 检查：element(失败抛出异常), peek(失败返回特殊值)

### BlockingQueue 实现类

* ArrayBlockingQueue：基于数组实现的有界阻塞队列，在创建时需要指定队列的容量大小，当队列已满时，进行入队操作的线程会被阻塞；当队列为空时，进行出队操作的线程会被阻塞。在生产者 - 消费者场景中，如果需要限制队列的大小，可以使用 ArrayBlockingQueue
* DelayQueue：无界阻塞队列，用于存放实现了 Delayed  接口的元素，只有当元素的延迟时间到期时，才能从队列中取出元素。如果需要按照元素的延迟时间来获取元素，如定时任务的场景，DelayQueue 是合适的选择。
* LinkedBlockingQueue：基于链表实现的可选有界阻塞队列（默认无界），如果在创建时不指定容量，则容量没有上限。当对队列的容量上限要求不明确或希望有较大的容量时，LinkedBlockingQueue可能更适用。

* SynchronousQueue：一个不存储元素的阻塞队列，每个插入操作必须等待另一个线程的移除操作，反之亦然，常用于实现线程之间的直接传递。在需要线程之间直接进行一对一传递而不需要存储元素时，SynchronousQueue 能发挥作用。