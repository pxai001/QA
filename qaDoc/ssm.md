## SpringMvc

1. 架构图

   * 前端控制器：DispatcherServlet，框架提供，负责总控，拦截用户请求
   * 处理器映射器：HandlerMapping，框架提供，负责根据总控传来的用户请求，获取处理器执行链（处理器和拦截器），返回总控处理器执行链
   * 处理器适配器：HandlerAdaptor，框架提供，负责根据总控传来的处理器执行链，找到处理器并执行获取ModelAndView，返回总控 ModelAndView
   * 处理器：Handler，一般指Controller，用户提供，负责执行具体业务并返回 ModelAndView
   * 视图解析器：ViewResolver，框架提供，负责根据总控的 ModelAndView  ，将逻辑地址转换为物理地址，并生成 View 对象，返回总控
   * 视图：View，一般指 jsp，thymleaf 页面，用户提供，负责根据总控返回 View 对象将数据渲染到页面上，并返回总控，最终展示到用户页面

2. 使用

   * xml <mvc:annotation-driven> 开启注解，<context:annotation-config/>也可以但不支持事务注解和aop注解

   - @RequestMapping 映射
   - @RequestBody 请求体封装
   - @ReponseBody 响应体封装
   - @RequestParam 请求参数封装
   - @Autowired
   - @Qualifier 和 @Autowired 搭配，可按照名字注入，相当于 @Resource
   - @Required  必须存在

3. 乱码问题

   * 修改tomat编码为utf-8；get先按页面编码转为字节，再用string转为utf8字符串
   * post 配置 CharacterEncodingFilter 过滤器

4. web.xml

   *  Servlet 3.0 以下

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns="http://java.sun.com/xml/ns/javaee"
            xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
            version="3.0">
   
       <!-- 配置 Spring 的监听器，用于初始化 Spring 上下文 -->
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:applicationContext.xml</param-value>
       </context-param>
   
       <!-- 配置 Spring MVC 的前端控制器 -->
       <servlet>
           <servlet-name>dispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:spring-mvc.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <servlet-mapping>
           <servlet-name>dispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <!-- 字符编码过滤器 -->
       <filter>
           <filter-name>encodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>UTF-8</param-value>
           </init-param>
           <init-param>
               <param-name>forceEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
       </filter>
   
       <filter-mapping>
           <filter-name>encodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   
   </web-app>
   ```

*  Servlet 3.0 以上(tomcat7以上)：实现 WebApplicationInitializer 接口即可

  ```java
  import org.springframework.web.WebApplicationInitializer;
  import org.springframework.web.context.ContextLoaderListener;
  import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
  import org.springframework.web.servlet.DispatcherServlet;
  
  import javax.servlet.ServletContext;
  import javax.servlet.ServletException;
  import javax.servlet.ServletRegistration;
  
  public class MyWebApplicationInitializer implements WebApplicationInitializer {
  
      @Override
      public void onStartup(ServletContext servletContext) throws ServletException {
          // 创建根上下文
          AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();
          rootContext.register(SpringConfig.class);  // 注册您的 Spring 配置类
  
          servletContext.addListener(new ContextLoaderListener(rootContext));
  
          // 创建 Spring MVC 的上下文
          AnnotationConfigWebApplicationContext mvcContext = new AnnotationConfigWebApplicationContext();
          mvcContext.register(SpringMvcConfig.class);  // 注册您的 Spring MVC 配置类
  
          ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcherServlet", new DispatcherServlet(mvcContext));
          dispatcher.setLoadOnStartup(1);
          dispatcher.addMapping("/");
      }
  }
  ```

  

## Spring

### 特性

1. 替代 EJB，java bean 管理更加方便
2. IOC 控制反转：不需要自己创建实例，通过工厂模式反射创建实例
3. DI 依赖注入：ioc 创建bean时调用set方法或构造器注入依赖的对象和成员变量
4. AOP 切面编程：底层为动态代理，通过切面来拦截类的方法，执行一些需要的操作
5. 设计模式：单例 bean默认为单例，策略 各种技术整合模版，工厂 创建bean
6. bean 生命周期：init-method,destroy-method 
7. 架构图
   * test
   * core：beans 实例工厂，core 底层和工具类，context 容器，el 表达式支持
   * aop，aspect
   * data：jdbc，orm
   * web：web，mvc，servlet
8. bean 作用域 scope：singleton，prototype，request，session，global-session

### 注解事务：@Transactional

1. 开启

   ```java
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
   </bean>
   <tx:annotation-driven transaction-manager="transactionManager" />
   @Transactional
   public void service() {
       // 事务相关的数据库操作
   }
   ```

   

2. 事务：

   * 原子性：事务内所有操作要么都完成要么都失败，失败要回滚；

   * 一致性：事务前后数据必须一致比如转账，

   * 隔离性：同一时间同一数据只有一个事务处理；

   * 持久性：事务执行后数据持久的保存再数据库，不会被回滚；

2. 事务隔离级别：

   * 读未提交，读了另一个事务未提交的数据，脏读，不可重复读，幻读

   * 读已提交，读了另一个事务修改前后的数据，不可重复读，幻读；oracle

   * 可重复读，读了另一个事务新增前后的数据，幻读；mysql

   * 串行化，一个事务操作数据，别的事务被挂起

3. 传播机制：propagation 默认 Required

   * REQUIRED：已有则用，没有新建

   * REQUIRES_NEW：总是新建，已有挂起

   * SUPPORTS：已有则用，没有不用

   * MANDATORY：已有则用，没有抛异常

   * NEVER：不用事务，已有抛异常

   * NOT_SUPPORTED：不用事务，已有挂起

   * NESTED：已有事务，嵌套执行

4. 事务级别：isolation 默认为数据库默认隔离级别， REPEATABLE_READ 可重复读；

5. 是否只读：readOnly

6. 异常处理：rollbackFor = Exception.class

### 注解 Aop

* @Aspect 切面
* @Pointcut("execution(\* \*(..))") 切入点
* 连接点：被拦截方法
* 目标对象：被拦截的对象
* 织入：将切面拦截对象
* @Before 前置通知
* @After 后置通知
* @AfterReturning 返回通知
* @AfterThrowing 异常通知
* @Around 环绕通知

## Mybatis

### 原生

 * 导入 mybatis 包
 * 核心配置文件：配置数据库，配置mapper位置
 * Mapper接口类，Mapper 配置文件
 * new SqlSessionFactoryBuilder().build(xmlInputStream).openSession
 * session.getMapper(dao.class);#
### 整合 spring

 * 导入 mybatis ，mybatis-spring 包

 * org.mybatis.spring.SqlSessionFactoryBean 设置数据库，主配置文件位置

 * org.mybatis.spring.mapper.MapperScannerConfigurer 设置 sqlSessionFactory，包扫描器

 * mapeer 文件：resultType，parameterType

 * 返回id 

   ```java
   // 自增 id
   useGeneratedKeys="true" 
   // oracle
   <selectKey resultType="INTEGER" order="BEFORE" keyProperty="userId"> 
   	SELECT SEQ_USER.NEXTVAL as userId from DUAL
   </selectKey>
   ```

## 缓存

* 一级缓存：作用域整个 session，session 关闭后清空，一次查询后就失效
* 二级缓存：作用域整个 mapper，可能会有脏数据，多次查询可共用
* 缓存更新：主表数据增删改后更新，因此如果关联表更新后缓存会有脏数据，尽量不用二级缓存