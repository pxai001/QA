## 自动配置原理

* @SpringbootApplication 底层SPI机制
* 加载了 META-INF/spring.factories 里的所有类
* debug：true 可以看到加载了哪些类

## 常用注解

* @Configuration 配置类
* 注册bean：@Bean @Component @ Controller @Service @Repository
* @ComponentScan @Import 扫描注册和手动注册
* @Conditional @ConditionalOnMissingBean(name = "tom") 满足指定条件注册
* @lmportResource 引入配置文件
* @SpringbootApplication springboot 启动类 
* @ConfigurationProperties(prefix = "mail")配置绑定
* @EnableConfigurationProperties (Car.class) 开启配置绑定并注册