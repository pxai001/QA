## 状态码

* 200 成功
* 301 url移走
* 302 重定向
* 400 请求错误
* 401 未授权
* 403 拒绝服务
* 404 资源不存在
* 500 服务器错误
* 503 暂时不能处理请求

## GET 和 POST

* get 不安全参数在链接上，且长度有限制，不同浏览器限制不一样
* post 安全参数在请求体中，长度无限制
* 请求转发地址不变，服务器内部转发；重定向地址改变，客户端改变

## cookie和session

* cookie 在客户端，只能存字符串类型
* session 在服务端，可以存任何类型
* session 共享：redis 存储 session或服务器session同步
  1. 重写服务器中的 HttpSession kv存在redis中
  2. 重写 HttpServletRequest，构造器传入原生 request， request.getSession 返回自定义 Session
  3. Filter 返回自定义 request
  4. 可使用

## Jsp 和 Servlet

1. Servlet 是运行在web服务器的Java程序，用来处理浏览器请求，用于构建web程序，java 类继承 HttpServlet 就是一个 servlet 实例
2. 当客户端发送一个 HTTP 请求到服务器时，Web 容器会根据请求的 URL 等信息找到对应的 Servlet 实例，并调用其相应的方法（如 `doGet` 、 `doPost` 等）来处理请求。Servlet 在处理请求过程中可以访问请求参数、会话信息、上下文等，然后通过输出流将生成的响应数据发送回客户端。
3. Jsp 允许页面嵌套java代码， 本质是 servlet，页面请求后会服务端转为servlet来处理
4. Jsp 四大域对象
   * pageContext 当前页面域
   * request 一次请求有效
   * session 一次会话有效
   * application context 服务器关闭前有效
5. Jsp 八大内置对象
   * Page：Jsp 本身
   * request：请求信息
   * reponse：响应信息
   * session：会话信息
   * Out 输出
   * PageContext 获取其他对象
   * Exception： 异常 < %@ page isErrorPage="true" %>
   * Application： 存储任何页面变量
   * Config： 配置信息

## Filter和Listener

1. 在 web.xml 里注册
2. Filter 过滤器，用来实现记录日志，过滤请求，编码转换
3. Listener 用于监听 Web 应用中的特定事件，并在事件发生时执行相应的处理逻辑。常见的事件包括：
   - ServletContext 相关事件 ServletContentListener：如应用的初始化和销毁。
   - HttpSession 相关事件 HttpSessionListener：如会话的创建、销毁、属性的添加、修改和删除。
   - ServletRequest 相关事件 ServletReuestListener：如请求的开始和结束。

## Ajax

1. 局部刷新，优化体验
2. 客户端运行，降低服务端压力

## 跨域

* 浏览器同源策略，相同协议，ip或域名，端口才可访问

* Jsonp：静态资源可跨域

  ```java
  客户端访问：
  <script src="https://example.com/api/data?callback=handleResponse"></script>
  服务器返回:myCallback({ "key": "value" });    
  ```

* 服务端设置相应头：

  ```java
  res.setHeader('Access-Control-Allow-Origin', '*');  // 允许所有源访问
  res.setHeader('Access-Control-Allow-Methods', '*');  // 允许的方法
  ```

  

  