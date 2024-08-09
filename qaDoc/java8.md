## Lambda，函数式接口，Stream

```java
new Thread( () -> System.out.println("In Java8, Lambda expression rocks !!") ).start()
  
list.forEach(n -> System.out.println(n));

public static void filter(List names, Predicate condition) {
	for(String name: names) {
		if(condition.test(name)) {
			System.out.println(name + " ");
		} 
  }
} 

names.stream().filter((name) -> (condition.test(name))).forEach((name) -> {
	System.out.println(name + " ");
});

list.stream().map((cost) -> cost + .12*cost).forEach(System.out::println);

List<String> filtered = strList.stream().filter(x -> x.length()> 2).collect(Collectors.toList());

List<Integer> distinct = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());

IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("Highest prime number in List : " + stats.getMax());
System.out.println("Lowest prime number in List : " + stats.getMin());
System.out.println("Sum of all prime numbers : " + stats.getSum());
System.out.println("Average of all prime numbers : " + stats.getAverage());
```

## Option 

### 基本

```java
//调用工厂方法创建 Optional 实例
Optional<String> name = Optional.of("Sanaulla");
//传入参数为 null，抛出 NullPointerException.
Optional<String> someNull = Optional.of(null);

//下面创建了一个不包含任何值的 Optional 实例,可以接受参数为 null 的
//例如，值为'null'
Optional empty = Optional.ofNullable(null);

//isPresent 方法用来检查 Optional 实例中是否包含值
Optional<String> name = Optional.of("Sanaulla");
if (name.isPresent()) {
//在 Optional 实例内调用 get()返回已存在的值
System.out.println(name.get());//输出 Sanaulla
}


Optional empty = Optional.ofNullable(null);
//在空的 Optional 实例上调用 get()，抛出 NoSuchElementException
System.out.println(empty.get());

//输出：There is no value present!
System.out.println(empty.orElse("There is no value present!"));

//输出：Default Value
System.out.println(empty.orElseGet(() -> "Default Value"));

empty.orElseThrow(ValueAbsentException::new);

Optional<String> name = Optional.of("Sanaulla");
//map 方法执行传入的 lambda 表达式参数对 Optional 实例的值进行修改。
//为 lambda 表达式的返回值创建新的 Optional 实例作为 map 方法的返回值。
Optional<String> upperName = name.map((value) -> value.toUpperCase());
System.out.println(upperName.orElse("No value found"));


Optional<String> name = Optional.of("Sanaulla");
//flatMap 与 map（Function）非常类似，区别在于传入方法的 lambda 表达式的返回类型。
//map 方法中的 lambda 表达式返回值可以是任意类型，在 map 函数返回之前会包装为 Optional。
//但 flatMap 方法中的 lambda 表达式返回值必须是 Optionl 实例。
upperName = name.flatMap((value) -> Optional.of(value.toUpperCase()));
System.out.println(upperName.orElse("No value found"));//输出 SANAULLA


Optional<String> name = Optional.of("Sanaulla");
//filter 方法检查给定的 Option 值是否满足某些条件。
//如果满足则返回同一个 Option 实例，否则返回空 Optional。
Optional<String> longName = name.filter((value) -> value.length() > 6);
System.out.println(longName.orElse("The name is less than 6 characters"));//输出 Sanaulla
//另一个例子是 Optional 值不满足 filter 指定的条件。
Optional<String> anotherName = Optional.of("Sana");
Optional<String> shortName = anotherName.filter((value) -> value.length() > 6);
//输出：name 长度不足 6 字符
System.out.println(shortName.orElse("The name is less than 6 characters"));
```

### 完整实例

```java
//创建 Optional 实例，也可以通过方法返回值得到。
Optional<String> name = Optional.of("Sanaulla");
//创建没有值的 Optional 实例，例如值为'null'
Optional empty = Optional.ofNullable(null);
//isPresent 方法用来检查 Optional 实例是否有值。
if (name.isPresent()) {
//调用 get()返回 Optional 值。
System.out.println(name.get());
}
try {
//在 Optional 实例上调用 get()抛出 NoSuchElementException。
System.out.println(empty.get());
} catch (NoSuchElementException ex) {
System.out.println(ex.getMessage());
}
//ifPresent 方法接受 lambda 表达式参数。
//如果 Optional 值不为空，lambda 表达式会处理并在其上执行操作。
name.ifPresent((value) -> {
System.out.println("The length of the value is: " + value.length());
});
//如果有值 orElse 方法会返回 Optional 实例，否则返回传入的错误信息。
System.out.println(empty.orElse("There is no value present!"));
System.out.println(name.orElse("There is some value!"));
//orElseGet 与 orElse 类似，区别在于传入的默认值。
//orElseGet 接受 lambda 表达式生成默认值。
System.out.println(empty.orElseGet(() -> "Default Value"));
System.out.println(name.orElseGet(() -> "Default Value"));
try {
//orElseThrow 与 orElse 方法类似，区别在于返回值。
//orElseThrow 抛出由传入的 lambda 表达式/方法生成异常。
empty.orElseThrow(ValueAbsentException::new);
} catch (Throwable ex) {
System.out.println(ex.getMessage());
}
//map 方法通过传入的 lambda 表达式修改 Optonal 实例默认值。
//lambda 表达式返回值会包装为 Optional 实例。
Optional<String> upperName = name.map((value) -> value.toUpperCase());
System.out.println(upperName.orElse("No value found"));
//flatMap 与 map（Funtion）非常相似，区别在于 lambda 表达式的返回值
//map 方法的 lambda 表达式返回值可以是任何类型，但是返回值会包装成 Optional 实例。
//但是 flatMap 方法的 lambda 返回值总是 Optional 类型。
upperName = name.flatMap((value) -> Optional.of(value.toUpperCase()));
System.out.println(upperName.orElse("No value found"));
//filter 方法检查 Optiona 值是否满足给定条件。
//如果满足返回 Optional 实例值，否则返回空 Optional。
Optional<String> longName = name.filter((value) -> value.length() > 6);
System.out.println(longName.orElse("The name is less than 6 characters"));
//另一个示例，Optional 值不满足给定条件。
Optional<String> anotherName = Optional.of("Sana");
Optional<String> shortName = anotherName.filter((value) -> value.length() > 6);
System.out.println(shortName.orElse("The name is less than 6 characters"));
```