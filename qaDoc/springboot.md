## 自动配置原理

* @SpringbootApplication 底层SPI机制
  * @SpringBootConfiguration ：表明这是一个 Spring Boot 的配置类，本质上它是 @Configuration 注解的派生注解，用于定义配置信息。
  * @EnableAutoConfiguration ：启用 Spring Boot 的自动配置功能。它会根据项目中的依赖和配置，自动配置很多常用的组件和功能，减少了手动配置的工作量
  * @ComponentScan ：用于扫描指定包及其子包下的组件（被 @Component 及其衍生注解标注的类），将其注册为 Spring Bean 并纳入 Spring 容器管理
* 加载了 META-INF/spring.factories 里的所有类
* debug：true 可以看到加载了哪些类

## 常用注解

* @Configuration 配置类
* 注册bean：@Bean @Component @ Controller @Service @Repository
* @ComponentScan @Import 扫描注册和手动注册
* @Conditional @ConditionalOnMissingBean(name = "tom") 满足指定条件注册
* @lmportResource 引入配置文件
* @SpringbootApplication springboot 启动类 
* 先@ConfigurationProperties(prefix = "mail")配置绑定，后在配置类上增加@EnableConfigurationProperties (Car.class) 开启配置绑定并注册