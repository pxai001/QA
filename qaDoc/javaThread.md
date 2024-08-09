## 线程类

```java
Thread thread = new Thread();
Thread threadRunable = new Thread(new RunableThread());
ExecutorService executorService = Executors.newFixedThreadPool(1);
Future<String> submit = executorService.submit(new CallableThread());
submit.get();

static class RunableThread implements Runnable {
  @Override
  public void run() {}
}

static class CallableThread implements Callable<String> {
  @Override
  public String call() throws Exception {
    return null;
  }
}
```

## 线程互斥，同步，通信

1. 主子任务：通过变量判断主子哪个执行；不满足条件 wait，满足条件执行完 notify
2. 共享数据：同一变量不同线程同时操作，操作方法加锁
3. 线程局部变量 ThreadLocal，set 将当前对象为键，值为传参；remove 释放内存
4. wait等待并释放锁，sleep睡眠不释放锁，notify释放锁唤醒（获取锁的线程由jvm决定），notifyAll释放锁唤醒（所有线程竞争获取锁）

## 线程池

```java
//创建固定大小的线程池
ExecutorService fPool = Executors.newFixedThreadPool(3);
//创建缓存大小的线程池
ExecutorService cPool = Executors.newCachedThreadPool();
//创建单一的线程池
ExecutorService sPool = Executors.newSingleThreadExecutor();

pool.execute(new Thread()) 执行

ScheduledExecutorService pool = Executors.newScheduledThreadPool(2);
pool.schedule(new Thread(), 10, timeunit); 周期定时执行
scheduleAtFixedRate (Runnable, long initialDelay, long period, TimeUnit timeunit); 上一个开始到下一个执行开始间隔
scheduleWithFixedDelay (Runnable, long initialDelay, long period, TimeUnit timeunit); 上一个结束到下一个执行结束间隔
    
Future future = executorService.submit(new Callable()); 返回值
future.get();

String result = executorService.invokeAny(callables); 多个返回，但只随机返回一个
List<Future<String>> futures = executorService.invokeAll(callables); 多个返回，返回所有

executorService.shutdown/shutdownNow(); 前者结束空闲线程，后者所有都结束
  
// 线程池执行者
ExecutorService threadPoolExecutor =
new ThreadPoolExecutor(
	corePoolSize,
	maxPoolSize,
	keepAliveTime,
	TimeUnit.MILLISECONDS,
	new LinkedBlockingQueue<Runnable>()
);
构造方法参数列表解释：
corePoolSize - 核心线程数
maximumPoolSize - 最大线程数
keepAliveTime - 超出核心线程数则进入阻塞队列，超出阻塞队列长度，增加新线程到最大线程数，增加的这部分如果空闲超出时间则销毁线程
unit - keepAliveTime 参数的时间单位。
workQueue - 任务

// 分支任务
public class MyRecursiveTask extends RecursiveTask<Long> {
  private long workLoad = 0;
  public MyRecursiveTask(long workLoad) {
  	this.workLoad = workLoad;
  }
  
  @abastract
  protected Long compute() {
    //if work is above threshold, break tasks up into smaller tasks
    if(this.workLoad > 16) {
      System.out.println("Splitting workLoad : " + this.workLoad);
      List<MyRecursiveTask> subtasks = new ArrayList<MyRecursiveTask>();
      subtasks.addAll(createSubtasks());
      for(MyRecursiveTask subtask : subtasks){
        subtask.fork();
      }
      long result = 0;
      for(MyRecursiveTask subtask : subtasks) {
        result += subtask.join();
      }
      return result;
    } else {
      System.out.println("Doing workLoad myself: " + this.workLoad);
      return workLoad * 3;
    } 
  }
  private List<MyRecursiveTask> createSubtasks() {
    List<MyRecursiveTask> subtasks = new ArrayList<MyRecursiveTask>();
    MyRecursiveTask subtask1 = new MyRecursiveTask(this.workLoad / 2);
    MyRecursiveTask subtask2 = new MyRecursiveTask(this.workLoad / 2);
    subtasks.add(subtask1);
    subtasks.add(subtask2);
    return subtasks;
  } 
}  
// 分支
ForkJoinPool forkJoinPool = new ForkJoinPool(4);
//创建一个没有返回值的任务
MyRecursiveAction myRecursiveAction = new MyRecursiveAction(24);
//ForkJoinPool 执行任务，execute 只提交无返回值；submit 提交，通过get获取返回值；invoke 提交 join 操作同步到主线程
Long res = forkJoinPool.invoke(myRecursiveAction);
```

## 定时器

```java
Timer timer = new Timer();
timer.schedule(new MyTimerTask() {
  @Override
  public void run() {
    count = (count +1)%2;
    System.err.println("Boob boom ");
    new Timer().schedule(new TimerTastCus(), 2000+2000*count);
  }
}, 1000);
```

## 死锁

1. 条件
   * 互斥：一个线程占有另一个只能等待
   * 不剥夺：其他线程不能抢占，只能本线程释放
   * 请求保持：已有资源，请求新资源但被占用，当前资源不释放，等待新资源释放
   * 循环等待：线程一等待线程二资源，线程二等待线程一资源
2. 避免
   * 加锁顺序一致：a和b资源都按同样顺序加锁
   * 加锁时限：获取不到锁就放弃当前资源
   * 避免锁嵌套
   * 预分配资源
