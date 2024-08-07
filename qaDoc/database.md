## 数据类型

1. 数值

   ![image-20220806193112068](/Users/songyujun/Library/Application Support/typora-user-images/image-20220806193112068.png)

2. 文本：

   ![image-20220806193231233](/Users/songyujun/Library/Application Support/typora-user-images/image-20220806193231233.png)

   * char 每次修改长度一样，效率高，但实际空间为预计空间+记录长度的字节，空间多；varchar 每次修改长度不一样，效率低，但实际空间为字符串空间+记录长度的字节，空间少；
   * nchar，nvarchar 占用两个字节，char，varchar占用一个字节

3. 日期

![image-20220806193217387](/Users/songyujun/Library/Application Support/typora-user-images/image-20220806193217387.png)

## SQL 语法

* like 模糊查询

* group by 分组统计，max，min，count，sum，avg 计算

* 统计数大于：先分组，再 having avg，count，sum

* 字段相同统计，分组，count

* 统计数排序：分组，having ，order by

* 日期条件：year(日期) = 2022 month(日期) = 10

* left join，right join，inner join

* 获取第几名：order by value limit 1,1 (第二页，每页一个)

* 分页：limit （page - 1）* pagesize，pagesize

* 插入修改触发

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
   * 多列索引：CREATE INDEX index ON table(filed1,filed2,filed3)，最左匹配原则使用频繁的列放到最左边，将会创建（1）（1,2）（1,2,3）索引，遇到（>,<,like,between）
   * 索引失效：索引列计算，字符串类型没有加引号， like, 没有最左索引列条件
2. 存储方式
   * b + 树

## 引擎

1. InnoDB
   * mysql 5.5.8 后默认
   * 支持事务
   * 索引更新行级锁，其他都是表锁
   * 5.7 后支持全文检索和空间函数