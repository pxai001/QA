## SpringMvc

1. 架构图

   * 前端控制器：DispatcherServlet，框架提供，负责总控，拦截用户请求
   * 处理器映射器：HandlerMapping，框架提供，负责根据总控传来的用户请求，获取处理器执行链（处理器和拦截器），返回总控处理器执行链
   * 处理器适配器：HandlerAdaptor，框架提供，负责根据总控传来的处理器执行链，找到处理器并执行获取ModelAndView，返回总控 ModelAndView
   * 处理器：Handler，一般指Controller，用户提供，负责执行具体业务并返回 ModelAndView
   * 视图解析器：ViewResolver，框架提供，负责根据总控的 ModelAndView  ，将逻辑地址转换为物理地址，并生成 View 对象，返回总控
   * 视图：View，一般指 jsp，thymleaf 页面，用户提供，负责根据总控返回 View 对象将数据渲染到页面上，并返回总控，最终展示到用户页面

2. 使用

   * xml <mvc:annotation-driven> 开启注解

   - @RequestMapping 映射
   - @RequestBody 请求体封装
   - @ReponseBody 响应体封装
   - @RequesParam 请求参数封装
   - @Autowired
   - @Qualifier 和 Autowired 搭配，可按照名字注入，相当于 @Resource
   - @Required  必须存在

## 注解事务

* 事务：原子性：事务内所有操作要么都完成要么都失败，失败要回滚；一致性：事务前后数据必须一致比如转账，隔离性：同一时间同一数据只有一个事务处理；持久性：事务执行后数据持久的保存再数据库，不会被回滚；
* 事务隔离级别：
  * 读未提交，读了另一个事务未提交的数据，脏读，不可重复读，幻读
  * 读已提交，读了另一个事务修改前后的数据，不可重复读，幻读；
  * 可重复读，读了另一个事务新增前后的数据，幻读；
  * 串行化，一个事务操作数据，别的事务被挂起
* @Transactional
* 传播机制：propagation 默认 Required
  * REQUIRED：已有则用，没有新建
  * REQUIRES_NEW：总是新建，已有挂起
  * SUPPORTS：已有则用，没有不用
  * MANDATORY：已有则用，没有抛异常
  * NEVER：不用事务，已有抛异常
  * NOT_SUPPORTED：不用事务，已有挂起
  * NESTED：已有事务，嵌套执行
* 事务级别：isolation 默认为数据库默认隔离级别， REPEATABLE_READ 可重复读；
* 是否只读：readOnly
* 异常处理：rollbackFor = Exception.class

## 注解 Aop

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

## Mybatic

 	1. 原生
 	 * 导入 mybatis 包
 	 * 核心配置文件：配置数据库，配置mapper位置
 	 * Mapper接口类，Mapper 配置文件
 	 * new SqlSessionFactoryBuilder().build(xmlInputStream).openSession
 	 * session.getMapper(dao.class);
 	2. 整合
 	 * 导入 mybatis ，mybatis-spring 包
 	 * org.mybatis.spring.SqlSessionFactoryBean 设置数据库，主配置文件位置
 	 * org.mybatis.spring.mapper.MapperScannerConfigurer 设置 sqlSessionFactory，包扫描器

## 