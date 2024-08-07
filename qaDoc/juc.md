## CAS

   ```java
   // java.util.concurrent.atomic
   AtomicBoolean.compareAndSet(false, true); // 比较并交换，用线程内存值和主内存值比较，如果相同则执行，否则自旋锁更新线程内存值直至相同
   AtomicInteger.getAndIncrement,incrementAndGet,getAndDecrement,decrementAndGet // 原子执行 ++ --，前者返回更新前后者返回更新后
   ```

## LOCK

```java
// java.util.concurrent.lock
Lock lock = new ReentrantLock(true);  默认 false 非公平锁；true 公平锁

Condition condition = lock.newCondition();
condition.await();
condition.signal();

Semaphore semaphore = new Semaphore(3, true);
semaphore.acquire();
semaphore.release();
```

## 阻塞队列

### BlockingQueue 方法
  * add 添加，offer，put，offer(obj, int timeunit, status)
  * remove
  * element 

### BlockingQueue 实现类

```java
ArrayBlockingQueue
DelayQueue
LinkedBlockingQueue
SynchronousQueue
```