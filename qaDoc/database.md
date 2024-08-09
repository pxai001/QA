## 数据类型

1. 数值

   - **MySQL**：`TINYINT`（1 字节，-128 到 127）、`SMALLINT`（2 字节，-32768 到 32767）、`MEDIUMINT`（3 字节）、`INT`（4 字节，-2147483648 到 2147483647）、`BIGINT`（8 字节）
   - **Oracle**：`NUMBER(3)`、`NUMBER(5)`、`NUMBER(10)` 等，可以指定整数的长度
   - **SQL Server**：`TINYINT`、`SMALLINT`、`INT`、`BIGINT`

   **浮点数类型：**

   - **MySQL**：`FLOAT`（单精度，4 字节）、`DOUBLE`（双精度，8 字节）
   - **Oracle**：`FLOAT`、`BINARY_FLOAT`（32 位单精度）、`BINARY_DOUBLE`（64 位双精度）
   - **SQL Server**：`REAL`（单精度，4 字节）、`FLOAT`（双精度，8 字节）

   **定点数类型：**

   - **MySQL**：`DECIMAL(M,D)`，M 表示总数字位数，D 表示小数位数
   - **Oracle**：`DECIMAL(M,D)`
   - **SQL Server**：`DECIMAL(M,D)`

2. 文本：

   - **MySQL**：`CHAR(n)`（固定长度字符串，n 为字符数）、`VARCHAR(n)`（可变长度字符串，n 为最大字符数）、`TEXT`（长文本）
   - **Oracle**：`CHAR(n)`、`VARCHAR2(n)`、`CLOB`（大型字符对象）
   - **SQL Server**：`CHAR(n)`、`VARCHAR(n)`、`TEXT`

   * char 每次修改长度一样，效率高，但实际空间为预计空间+记录长度的字节，空间多；varchar 每次修改长度不一样，效率低，但实际空间为字符串空间+记录长度的字节，空间少；
   * nchar，nvarchar 占用两个字节，char，varchar占用一个字节

3. 日期时间

   - **MySQL**：`DATE`（日期，格式：'YYYY-MM-DD'）、`TIME`（时间，格式：'HH:MM:SS'）、`DATETIME`（日期和时间，格式：'YYYY-MM-DD HH:MM:SS'）、`TIMESTAMP`（时间戳）
   - **Oracle**：`DATE`（包含日期和时间，精度到秒）、`TIMESTAMP`（更高精度的时间戳）
   - **SQL Server**：`DATE`、`TIME`、`DATETIME`、`DATETIME2`（更高精度）

4. 布尔类型

   - **MySQL**：`BOOL` 或 `BOOLEAN`（实际上以 `TINYINT` 存储，0 表示假，1 表示真）
   - **Oracle**：`BOOLEAN`
   - **SQL Server**：`BIT`（0 或 1）

## SQL 语法

* like 模糊查询

* group by 分组统计，max，min，count，sum，avg 计算

* 统计数大于：先分组，再 having avg，count，sum

* 字段相同统计，分组，count

* 统计数排序：分组，having ，order by

* 日期条件：year(日期) = 2022 month(日期) = 10

* left join 左全部，right join 右全部，inner join 左右交集

* 获取第几名：order by value limit 1,1 (第二页，每页一个)

* 分页：limit （page - 1）* pagesize，pagesize

* 顺序：from join on  where group by  with 聚合 having 计算 select distinct order by

* 创建表：create table name {}

* 修改表：alter table tbName add/modify column/drop column colName colType

* 插入表修改触发

  ```sql
  create_time datetime default CURRENT_TIMESTAMP(3) null comment '创建时间',
  update_time datetime default CURRENT_TIMESTAMP(3) null on update CURRENT_TIMESTAMP(3) comment '更新时间',
  ```

## Mysql

## 结构

* 连接层：处理编程语言连接
* 服务层：连接池管理连接，系统管理，解析执行 SQL，查询优化，缓存，与存储引擎优化
* 引擎层：数据存储和提取，与底层系统文件交互
* 存储层：存储到文件系统，日志文件，配置文件，数据文件

## 索引 

1. 类型
   * 普通索引：提高访问速度，CREATE INDEX index ON table(filed)
   * 唯一索引：避免重复，CREATE UNIQUE INDEX index ON table(filed)
   * 主键索引：唯一不为空，PRIMARY KEY
   * 空间索引：空间数据类型索引，CREATE SPATIAL INDEX index ON table(filed)
   * 全文检索：char，varchar，text 类型索引，查找文本中的关键字，MyISAM 引擎支持，允许重复值和空值，CREATE FULLTEXT INDEX index ON table(filed）
   * 联合索引：CREATE INDEX index ON table(filed1,filed2,filed3)，最左匹配原则使用频繁的列放到最左边，将会创建（1）（1,2）（1,2,3）索引
2. 存储方式
   * b + 树

## Sql 优化

1. 只要一行数据 limit 1，否则会继续查询是否匹配
2. 建立索引
   * where，order by 字段建立索引
   * 最左匹配原则：最左匹配原则使用频繁的列放到最左边，将会创建（1）（1,2）（1,2,3）索引
3. 避免索引失效：避免不利于索引关键字：in（用 exists(sql)替代），not in（用 not exists(sql) 替代）， is null，is not null, like, between 
4. 包含数值的尽量使用数值类型，字符类型匹配性能低
5. 避免 select * ，即便所有列也需要全部列出

## 引擎

1. MyIsam
   * 不支持事务
   * 表锁
   * 操作速度快
2. InnoDB
   * mysql 5.5.8 后默认
   * 支持事务
   * 索引更新行级锁，其他都是表锁
   * 5.7 后支持全文检索和空间函数

## Oracle

### SQL

1. 字符串连接 ||
2. 分页 rownum

## 事务隔离级别

* 读未提交：读了未提交事务的数据，脏读，不可重复读，幻读
* 读已提交：无法读到还未提交的更新数据，不可重复读，幻读，oracle 默认
* 可重复读：无法知道到之后新加的数据，幻读，mysql 默认
* 串形化：读取时不能数据增删改，效率低