## 四舍五入

```java
Math.round(); // 取整
BigDecimal bd = new BigDecimal(d).setScale(3, BigDecimal.ROUND_HALF_UP).doubleValue(); // 保留小数
String.format("%.2f", 2.565); // 输出:2.57，保留小数
```

## String

1. string.length(); 长度

2. string.indexOf(''); 第一次出现

3. string.lastIndexOf(''); 最后一次出现

4. string.subString(1); 1到结束截取

5. string.subString(0, 1); 0到1截取

6. string.chatAt(1); 1处的字符

7. String 不可变，StringBuffer 可变线程安全, StringBuilder 可变线程不安全

8. 内存地址

   ```java
   String s1 = "Programming";
   String s2 = new String("Programming");
   String s3 = "Program";
   String s4 = "ming";
   String s5 = "Program" + "ming";
   String s6 = s3 + s4;
   System.out.println(s1 == s2); //false new为新对象
   System.out.println(s1 == s5); //true 常量相加对应同一常量
   System.out.println(s1 == s6); //false 变量相加为新对象
   System.out.println(s1 == s6.intern()); //true 常量池引用相同
   System.out.println(s2 == s2.intern()); //false new为新对象，和常量池对象引用不一样
   ```

   

## Clone

1. 浅拷贝：实现 Cloneable 接口的clone方法调用Object对象super.clone()，调用clone();
2.  深拷贝：重写clone方法，将对象用对象输出流写入字节输出流，再用对象输入流从字节流输入流里读出来
3. clone和new区别：new分配内存根据类型分配，调用构造方法填充对象成员变量；clone分配原对象内存大小，用原对象填充对象成员变量；

## HashCode

1. equals相同hashCode一定相同，hashCode相同equals不一定相同
2. 重写equals：满足自反性，对称性，传递性，一致性；==，instaceof为true，保证hashcode相等

## Error 和 Exception

1. Error 为系统错误，虚拟机错误等无法处理的异常只能中断程序；Exception 为可处理的异常分为运行时异常和编译时异常，运行时异常可在运行时抛出或捕获，编译时异常（受检）需要在编译前处理
2. 常见运行时异常：
   * 空指针异常：NullPointer
   * 方法找不到异常：NoSuchMethod
   * 数组角标越界异常：IndexOutOfBounds
   * 类型转换异常：ClassCast
   * 数据类型转换异常：NumberFormat
3. 常见编译时（受检）异常
   * IO异常：IOException
   * SQL异常：SQLException
   * 类找不到异常：ClassNotFound
   * 文件找不到异常：FileNotFound

## Throw 和 Throws

1. throw 抛出异常，throws 声明异常
2. try catch：throws 的异常需要捕获， finally：发生异常时必须执行的代码块 

