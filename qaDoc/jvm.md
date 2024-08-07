## 反射

* 获取字节码Class.forName,xxx.class,this.getClass

* 获取方法，字段，构造函数

  ```java
  getConstructor(Class... param) // 公有构造
  
  getMethod(String methodName, Class... param) // 获取方法
  
  getField(String filedName)
  
  ```

* 代理

  ```java
  静态代理：代理类实现目标类相同接口，然后传入目标类对象，实现方法体为目标类对象调用自己的方法和其他增强方法
    
  JDK 动态代理：利用反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理
  Proxy.newProxyInstance(obj.getClass.getClassLoader,obj.getClass().getInterfaces,invoke)
    
  GBLIB 动态代理：利用asm开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。
  package com.cglib;
   
  import java.lang.reflect.Method;
   
  import net.sf.cglib.proxy.Enhancer;
  import net.sf.cglib.proxy.MethodInterceptor;
  import net.sf.cglib.proxy.MethodProxy;
   
  public class ProxyFactory implements MethodInterceptor{
   
      private Object target;//维护一个目标对象
      public ProxyFactory(Object target) {
          this.target = target;
      }
      
      //为目标对象生成代理对象
      public Object getProxyInstance() {
          //工具类
          Enhancer en = new Enhancer();
          //设置父类
          en.setSuperclass(target.getClass());
          //设置回调函数
          en.setCallback(this);
          //创建子类对象代理
          return en.create();
      }
   
      @Override
      public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
          System.out.println("开启事务");
          // 执行目标对象的方法
          Object returnValue = method.invoke(target, args);
          System.out.println("关闭事务");
          return null;
      }
  }
  ```

## 类加载器

1. 引导类加载器( /jre/lib/ 原生C语言底层代码），扩展类加载器（jre\lib\ext  扩展库代码），系统类加载器（classpath 自己写的java），自定义类加载器
2. 加载器双清委派机制：从上向下加载类
3. 类初始化条件：新建实例，访问静态变量，调用静态方法，子类初始化，jvm 启动类
4. 类初始化步骤：加载，初始化父类，静态变量和代码块
5. jvm 加载 Class：java 编译为 class，加载到内存，读取class文件，创建class文件对象
   * 加载：类加载器加载类，双清委派机制
   * 验证：验证类语法是否正确
   * 准备：分配内存
   * 解析：常量池符号引用改为直接引用
   * 类初始化：父类静态变量和代码块 =>子类静态变量和静态代码块
   * 实例化：堆内存分配空间，栈内存引用变量，父类成员变量和代码块=>成员变量和代码块

## 内存结构

1. 方法区

   * 常量池：常数，String 字符串
   * 静态变量
   * 加载类编译后的信息；反射创建的类信息

2. 栈

   * 基本类型
   * 形参
   * 引用变量
   * 方法调用方法帧

3. 堆

   * 引用类型

4. 引用

   ```
   1. String abc=new String("abc"); // 强
   2. SoftReference<String> softRef=new SoftReference<String>(abc); //软
   3. WeakReference<String> weakRef = new WeakReference<String>(abc); //虚 
   4. abc=null; //4 软可及
   5. softRef.clear();// 弱引用
   ```

   * 堆内存
   * 强引用：不回收，内存不够抛内存不足异常，new出来的都是
   * 软引用：内存不够回收
   * 弱引用：扫描到就回收
   * 虚引用：任何时候都会被回收

5. 堆和栈的区别

   * 栈系统自动分配内存，堆内存需要申请
   * 栈内存超出溢出异常，堆从空闲内存地址链表申请
   * 栈是连续空间，堆是不连续空间

## GC 

1. 位置
   * 堆空间：新生代（8:1:1，Eden，Survivor：from，to），老年代
   * 新生代：对象新建时在 Eden 或 survivor from，Eden 内存不够触发 Minor GC， GC后存活的到 Survivor to，然后清除 Survivor from 和 Eden，最后互换 from 和 to 的作用
   * 老年代：新生代存活一次，年龄 +1，当达到 一定阈值（默认15） 时，会放到老年代；如果老年代连续空间小于对象总大小，出发 full gc，full gc 效率慢
   * 永久代：方法区规范的实现方式
   * 元空间： metaspace jdk8引入，jdk8 前用永久代，由于类及方法信息无法确定，太小容易溢出，太大占用了虚拟机内存其他空间；metaspace 使用本地内存，不受虚拟机内存限制
2. 回收
   * 回收无用的垃圾内存引用
   * 搜索算法：引用计数，（废弃）计数为0则为垃圾；根搜索法，无子节点则为垃圾
   * 回收算法：
     1. 标记清除：标记回收；会有不连续空间，浪费；
     2. 复制算法：存活的放到内存一半，清除另一半；每次只使用一半，利用率低；大部分都是朝生夕死，现在 JVM 8:1:1
     3. 标记整理：存活的移到一边，适合老年代
     4. 分代收集：新生代复制算法，老年代标记整理算法
3. 分类

   * partial GC: 代表局部垃圾回收
   * young GC（Minor GC）： 指的是对新生代区域垃圾回收
   * old GC：指的是收集老年代，只有 CMS 的 concurrent collection是这个模式
   * Mixed GC：收集整个新生代，和部分老年代，只有G1有这个模式
   * Full GC：收集整个过程，包括新生代、老年代、永久代（JDK8之前）
   * Major GC：可以是指 old GC 也可以是指 Full GC。这是因为JVM规范没有对这些名词有具体的定义，时间久了后就使用混乱了。所以如果讨论 Major GC 的时候需要指定前提到底是讨论的是 old GC 还是 Full GC，比如周志明版的《深入理解虚拟机》就把 old GC 统称为 【Major GC / Full GC】更加不做区分了，这里我们先按照 old GC == Major GC 讨论

## Jvm 内存调优

```shell
java -Xmx3550m -Xms3550m -Xss128k -XX:NewRatio=4 -XX:SurvivorRatio=4 -XX:MaxPermSize=16m -XX:MaxTenuringThreshold=0
-XX:NewRatio=4:设置年轻代（包括Eden和两个Survivor区）与年老代的比值（除去持久代）。设置为4，则年轻代与年老代所占比值为1：4，年轻代占整个堆栈的1/5
-XX:SurvivorRatio=4：设置年轻代中Eden区与Survivor区的大小比值。设置为4，则两个Survivor区与一个Eden区的比值为2:4，一个Survivor区占整个年轻代的1/6
-XX:MaxPermSize=16m:设置持久代大小为16m。
-XX:MaxTenuringThreshold=0：设置垃圾最大年龄。如果设置为0的话，则年轻代对象不经过Survivor区，直接进入年老代。对于年老代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则年轻代对象会在Survivor区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概论。
```

* java -Xmx 最大内存

* java -Xms 初始内存，默认物理内存1/64；一般和 xmx 一样性能会好

* java -Xmn 年轻代内存，推荐堆内存 3/8

* -XX:PermSize=128m：持久代内存初始值分配128M

* -XX:MaxPermSize=512m：设置持久代最大为512m

* java -XX:MetaspaceSize ：元空间大小

* java -Xss 每个线程堆栈大小

* -XX:+PrintGC 每次触发 GC 的时候打印相关日志

* -XX:+PrintGCDetails 更详细的 GC 日志

* -XX:MaxTenuringThreshold：进入老年代阈值设置

* -XX:SurvivorRatio：幸存者比例设置，设置年轻代中Eden区与Survivor区的大小比值。设置为8，则两个Survivor区与一个Eden区的比值为2:8，一个Survivor区占整个年轻代的1/10

* -XX:NewRatio：新生代比例设置（包括Eden和两个Survivor区）与年老代的比值（除去持久代）。设置为1，则年轻代与年老代所占比值为1：1，年轻代占整个堆栈的1/2。

* -Xms：初始堆内存大小，设定程序启动时占用内存大小，默认物理内存1/64   -Xms = -XX:InitialHeapSiz

## Jvm 回收器

* -XX:+UseSerialGC，单线程串行回收，jdk1.5 默认，回收程序停止时间较长，阻塞，适合小型应用

  ```
  新生代、老年代使用串行回收；新生代复制算法、老年代标记-压缩；垃圾收集的过程中会Stop The World（服务暂停）
  ```


* -XX:+UseParNewGC ParNew：多线程并行回收

  ```
  新生代并行，老年代串行；新生代复制算法、老年代标记-压缩
  ```


* XX:+USeParNewGC:parallel,类似 ParNew，效率高，适合大数据收集

  ```
  新生代复制算法、老年代标记-压缩
  ```


* -XX:+UseConcMarkSweepGC，cms收集器:标记清除，适合大型服务器


* -XX:+UseG1GC，g1 收集器，高吞吐量