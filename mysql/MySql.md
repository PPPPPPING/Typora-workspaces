[toc]

# 怎么设计好的索引

# 幻读

# mysql怎么实现分布式锁

## 具体的功能开发怎么做的

# 订单类型有很多吗 就是订单逻辑

# 怎么处理标准的下单逻辑和个性化的需求

# 长连接相关的工作，知道netty，websocket

# TCP协议的验包拆包了解吗 为什么要拆包，具体怎么做

# SQL优化，有什么注意事项

# 有没有分库分表

# or关键字 索引什么时候失效

# 为什么会出现回表

# 表分页的优化

# mysql引擎的区别

# 事务的四个特性详细解释

# 传播行为，默认是哪个模式，rollback那个属性默认回滚的是哪个异常

# 主键索引和二次索引

# 存储引擎对比

# 底层时如何组织存储的 

# 索引 

# buffer 

# Pool 

# bin log 

# redo log 

# undo log 

# 事务隔离级别

  

# mvcc 

# 锁 

# explian

# 索引

索引是对数据库表中一列或多列的值进行排序的一种结构。MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。

主键索引：一张表只能有一个主键索引，不允许重复、不允许为 NULL；

```sql
 ALTER TABLE TableName ADD PRIMARY KEY(column_list); 
```

唯一索引：数据列不允许重复，允许为 NULL 值，一张表可有多个唯一索引，索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。

```sql
CREATE UNIQUE INDEX IndexName ON `TableName`(`字段名`(length));
# 或者
ALTER TABLE TableName ADD UNIQUE (column_list); 
```

普通索引：一张表可以创建多个普通索引，一个普通索引可以包含多个字段，允许数据重复，允许 NULL 值插入；

```sql
CREATE INDEX IndexName ON `TableName`(`字段名`(length));
# 或者
ALTER TABLE TableName ADD INDEX IndexName(`字段名`(length));
```

聚集索引

![image-20240416080402958](https://github.com/PPPPPPING/Typora-workspaces@master/img/image-20240416080402958.png)

![image-20240416080617857](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416080617857.png)

# 事务

![image-20240417080754745](/Users/chenxi/Library/Application Support/typora-user-images/image-20240417080754745.png)

数据库的一次操作从开始到结尾，其中出现问题所有的数据会回滚

# 事务隔离级别

脏读

# MyBatis,Mybatisplus区别

如果Mybatis Plus是扳手，那Mybatis Generator就是生产扳手的工厂。
MyBatis：一种操作数据库的框架，提供一种Mapper类，支持让你用java代码进行增删改查的数据库操作，省去了每次都要手写sql语句的麻烦。但是！有一个前提，你得先在xml中写好sql语句，也是很麻烦的。
MP的存在就是为了稍稍弥补Mybatis的不足。在我们使用Mybatis时会发现，每当要写一个业务逻辑的时候都要在DAO层写一个方法，再对应一个SQL，即使是简单的条件查询、即使仅仅改变了一个条件都要在DAO层新增一个方法，针对这个问题，MP就提供了一个很好的解决方案，它可以让我们避免许多重复性的工作。

# ACID

![image-20240416041101994](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416041101994.png)

### 原子性

undolog,sql报错会会滚

https://www.cnblogs.com/kismetv/p/10331633.html

# 行锁升级为表锁

![image-20240416042957000](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416042957000.png)

![image-20240416043126076](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416043126076.png)

所以可以将uqdate改为select语句,explain看执行计划,看是否走索引,如果没走,就会行锁升级为表锁

# 百万级别数据库优化

![image-20240415182528593](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415182528593.png)
![image-20240415182737082](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415182737082.png)
![image-20240415182811627](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415182811627.png)

尽量不要写子查询

![image-20240415192758013](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415192758013.png)

![image-20240415192701198](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415192701198.png)

![image-20240415193709199](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415193709199.png)

普通索引:

![image-20240415194049458](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415194049458.png)

复合索引:

![image-20240415193957421](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415193957421.png)

![image-20240415194218655](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415194218655.png)

范围索引放最后

like ‘aa%’ 走索引, like ‘%aa’ 不走索引, 覆盖索引放最后

查询压力特别大, 就放入redis缓存

写操作压力大就用mq限流

# 内连接和外连接的区别

```sql
SELECT
	*
FROM
	teacher
LEFT JOIN course ON course.Tid = teacher.Tid

SELECT
	*
FROM
	course
RIGHT JOIN teacher ON course.Tid = teacher.Tid

SELECT
	*
FROM
	teacher
JOIN course ON course.Tid = teacher.Tid
```
