## Jdbc 过程

* 加载驱动：Class.forName
* 获取数据库连接对象： conn = DriverManager.getConnection('jdbc:mysql://localhost:3306/db',name,password) 
* 设置sql：statement = conn.createPreparedStatement(sql);statement.setObj(0,'0');
* 执行sql，获取结果集：resultSet = statement.executeQuery/execute/executeUpdate;
* 关闭结果集resultSet，关闭statement，关闭连接conn

## PreparedStatement 和 Statement

* PreparedStatement 继承 Statement
* 可防止sql注入
* 编译过的语句有缓存机制

## 数据库连接池

* 对象池思想
* 最大连接数
* 最小连接数
* 连接超时时间