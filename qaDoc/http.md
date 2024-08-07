1. 状态码
   * 200 成功
   * 301 url移走
   * 302 重定向
   * 400 请求错误
   * 401 未授权
   * 403 拒绝服务
   * 404 资源不存在
   * 500 服务器错误
   * 503 暂时不能处理请求
2. GET 和 POST
   * get 不安全参数在链接上，且长度有限制，不同浏览器限制不一样
   * post 安全参数在请求体中，长度无限制
3. 请求转发地址不变，服务器内部转发；重定向地址改变，客户端改变
4. cookie session
   * cookie 在客户端，只能存字符串类型
   * session 在服务端，可以存任何类型
   * session 共享：redis 存储 session或服务器session同步
     1. 重写服务器中的 HttpSession kv存在redis中
     2. 重写 HttpServletRequest，构造器传入原生 request， request.getSession 返回自定义 Session
     3. Filter 返回自定义 request
     4. 可使用