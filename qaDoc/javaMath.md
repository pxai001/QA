## 四舍五入

```java
Math.round(); // 取整
BigDecimal bd = new BigDecimal(d).setScale(3, BigDecimal.ROUND_HALF_UP).doubleValue(); // 保留小数
String.format("%.2f", 2.565); // 输出:2.57，保留小数
```