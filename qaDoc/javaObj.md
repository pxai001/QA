## 下载地址
```
https://repo.huaweicloud.com/java/jdk/8u202-b08/
https://www.oracle.com/ca-fr/java/technologies/javase/javase8-archive-downloads.html
```
## 文档
```
 https://docs.oracle.com/javase/8/docs/api/index.html
```
## 面向对象

1. 面向对象思想，C++ 1979年，Java 1995年，Java 摒弃了指针，增加了垃圾回收更好用;

   面向过程分步骤完成任务性能高耦合度高，面向对象封装任务步骤低耦合但性能低

2. 继承：从已有类继承信息创建新类，新类为子类，已有类为父类；使软件系统有了延续性，继承修饰符 public(全部类)>protected(本类，本包，子类)>default(本包，本类)>private(本类)

3. 封装：将对象的属性和操作属性的方法封闭到类中，对外统一通过类使用

4. 多态：同一父类或接口引用运行同一方法，得到不同的结果；运行时多态重写是多态重要部分，编译时多态重载；

5. 抽象：提取同一类的公共属性，abstract 修饰类和方法

6. 内部类：new Out().new Inner();

## 多态类型

1. 运行时多态：重写，后绑定
2. 编译时多态：重载，前绑定
3. 重写：父类和子类中，方法名，方法参数必须一样，异常不能大于父类，修饰符不能小于父类
4. 重载：同一个类中，除方法名一样，参数数量，顺序，类型，异常和修饰符都可以不一样

## 抽象和接口区别

1. 抽象修饰符 abstract 接口修饰符 interface
2. 抽象可以有构造器，具体方法，静态方法，变量；接口都是抽象方法，只有常量（public static final 变量），不能静态方法，java 8 可以有默认和静态方法；
3. 抽象只能单继承，接口可以多实现；
4. 都不能实例化，都可以作为多态引用，都需要全部实现；

## 数据类型

1. byte：1字节 8位 -128～127
2. short：2字节 16位 正负3万多；short s1 = 1（错，1是int无法转换）; s1 += 1 （对，自动转换类型为short）
3. int：4字节 32位 正负21亿
4. float：4字节 32位 
5. double：8字节 64位
6. long：8字节 64位 正负百万亿以上
7. boolean：1字节
8. char：2字节 8位
9. 包装对象：int为Integer，char为Character，其余首字母大写，java5后自动拆箱；Integer -128到127会使用同一地址；
