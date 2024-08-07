1. jdbc 过程
   * Class.forName 加载驱动（jdk1.6后无需加载）
   * conn = DriverManager.getConnectim 获取数据库连接对象
   * conn.createStatement(); conn.createPreparedStatement
   * 执行sql
   * 获取结果集
   * 关闭结果集，关闭会话，关闭连接
2. PreparedStatement 和 Statement
   * PreparedStatement 继承 Statement
   * 可防止sql注入
   * 编译过的语句有缓存机制
3. 数据库连接池
   * 对象池思想
   * 最大连接数
   * 最小连接数
   * 连接超时时间