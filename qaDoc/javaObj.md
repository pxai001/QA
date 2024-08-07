## 下载地址
```
https://repo.huaweicloud.com/java/jdk/8u202-b08/
https://www.oracle.com/ca-fr/java/technologies/javase/javase8-archive-downloads.html
```
## 文档
```
 https://docs.oracle.com/javase/8/docs/api/index.html
```
## 三大特征

1. c++ 1979年，java 1995年，但 java 摒弃了指针更好用
2. 继承：从已有类继承信息创建新类，新类为子类，已有类为父类，public>protected>default>private
3. 封装：将对象的属性和操作属性的方法封闭到类中
4. 多态：同一父类或接口引用执行同一方法，得到不同的效果
5. 抽象：同一类的公共属性，abstract 只能修饰类和方法

## 多态类型

1. 运行时多态：重写，后绑定
2. 编译时多态：重载，前绑定
3. 重写：父类和子类中，方法名，方法参数必须一样，异常不能大于父类，修饰符不能小于父类
4. 重载：同一个类中，除方法名一样，参数数量，顺序，类型，异常和修饰符都可以不一样

## 抽象和接口区别

1. 抽象 abstract 接口 interfact
2. 抽象可以有构造器，具体方法，静态方法，成员变量，接口只能有非静态 public或protected抽象方法，final 变量
3. 抽象只能单继承，接口可以多实现