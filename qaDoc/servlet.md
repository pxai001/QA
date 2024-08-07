1. Servlet 原理
   * Servlet 是web服务器上的组件，由服务器servlet引擎执行
   * Jsp 是特殊的 Servlet，web 服务器收到 jsp 请求会将其转换为 servlet 然后再执行
2. Jsp 四大域对象
   1. pageContext 当前页面域
   2. request 一次请求有效
   3. session 一次会话有效
   4. application context 服务器关闭前有效
3. Jsp 八大内置对象
   * Page：Jsp 本身
   * request：请求信息
   * reponse：响应信息
   * session：会话信息
   * Out 输出
   * PageContext 获取其他对象
   * Exception： 异常 < %@ page isErrorPage="true" %>
   * Application： 存储任何页面变量
   * Config： 配置信息