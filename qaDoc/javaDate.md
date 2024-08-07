## 格式化

```java
SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.sss");
Date date = new Date();
oldFormatter.format(date);
// Java 8
DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.sss");
LocalDateTime date = LocalDateTime.now();
date2.format(newFormatter);
```

## 解析

```java
String dateStr = "2022-01-01 08:00:00.000";
SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.sss");
Date oldDate = oldFormatter.parse(dateStr);

// java8
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