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