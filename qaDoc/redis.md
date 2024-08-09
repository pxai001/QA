## 安装

* 地址：https://redis.io/download/

## 数据类型

* string（字符串）: kv 都是字符串，适合做缓存，避免缓存穿透：设置合适的缓存策略避免同一时间失效；查询为空存储 null 值；布隆过滤器

  ```
  SET k v;
  GET k
  ```

  

* hash（哈希）：k是字符串，v 是kv，适合存储对象

  ```
  HSET k vk1 v1 vk2 v2
  HGET k vk1
  ```

  

* list（列表）：相当于一个队列，可以在头部和尾部添加元素

  ```
  左添加：lpush k v1 
  存在时左添加：lpushx
  左弹出：lpop k
  右添加：rpush
  存在时右添加：rpushx
  右弹出：rpop 
  ```

* set（集合）：无序集合

  ```
  添加：sadd k v
  返回所有：smembers k
  ```

  

* zset(sorted set)：有序集合

  ```
  添加并赋予分数，分数可重复：zadd key v score
  ```

  

## 备份和恢复

* RDB：指定时间间隔生成快照
  * 每小时，每一天，子进程处理快照工作
  * 间隔期间可能存在丢失数据
* AOF：记录操作命令，启动时使用命令还原
  * 可以频率为每秒 fsync 策略，每次追加，停止运行会新建文件
  * 相对 rdb 体积过大

## 整合 springboot

* maven

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-redis</artifactId>
  </dependency>
  ```

* yml

  ```
  redis:
  	database:
  	host:
  	port:
  	password:
  	timeout:
  	lettuce:
  ```

  

## 原子性

* API 单线程操作保持原子性
* 多个命令同时执行，先读，再写，再读
  * INCR，DECR
  * LUA 脚本
* Redistion