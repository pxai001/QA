## 获取当前时间

```java
Calendar cal = Calendar.getInstance();
System.out.println(cal.get(Calendar.YEAR));
System.out.println(cal.get(Calendar.MONTH)); // 0 - 11
System.out.println(cal.get(Calendar.DATE));
System.out.println(cal.get(Calendar.HOUR_OF_DAY));
System.out.println(cal.get(Calendar.MINUTE));
System.out.println(cal.get(Calendar.SECOND));
// Java 8
LocalDateTime dt = LocalDateTime.now();
System.out.println(dt.getYear());
System.out.println(dt.getMonthValue()); // 1 - 12
System.out.println(dt.getDayOfMonth());
System.out.println(dt.getHour());
System.out.println(dt.getMinute());
System.out.println(dt.getSecond());

// 毫秒
Calendar.getInstance().getTimeInMillis(); //第一种方式
System.currentTimeMillis(); //第二种方式
// Java 8
Clock.systemDefaultZone().millis();
```



## 格式化

```java
// Java7
SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.sss");
Date date = new Date();
oldFormatter.format(date);
// Java8
DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.sss");
LocalDateTime date = LocalDateTime.now();
date2.format(newFormatter);
```

## 解析

```java
// Java7
String dateStr = "2022-01-01 08:00:00.000";
SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.sss");
Date oldDate = oldFormatter.parse(dateStr);
// Java8
DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.sss");
LocalDateTime localDateTime = LocalDateTime.parse(dateStr, newFormatter);
// 转换
Date date = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
LocalDateTime localDateTime = new Date().toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
```

## 其他

```java
LocalDate today = LocalDate.now();
// 闰年
today.isLeapYear(); 
// 前后
today.isBefore(LocalDate.of(2023,1,1));
today.isAfter(LocalDate.of(2023,1,1));
// 加减时间
today.plusDays(10);
today.plusWeeks(3);
today.plusMonths(20);
today.minusDays(10);
today.minusWeeks(3);
today.minusMonths(20);
// 本月第一天
today.with(TemporalAdjusters.firstDayOfMonth());
// 本月最后一天
LocalDate lastDayOfYear = today.with(TemporalAdjusters.lastDayOfYear());
// 日期区间
Period period = today.until(lastDayOfYear);
period.getMonths();
```