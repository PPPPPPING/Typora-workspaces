redis-server /opt/homebrew/etc/redis.conf 启动

![image-20240324194048013](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324194048013.png)

不要限制ip

![image-20240324194213984](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324194213984.png)

远程保护关闭，表示谁都可以远程登录

![image-20240324194614959](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324194614959.png)

AOF默认关闭，现在需要打开

![image-20240324194753655](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324194753655.png)

always 每时每刻
everysec 每秒
不同步

![image-20240324194849792](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324194849792.png)

RDB和AOF混合模式

![image-20240324195609689](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324195609689.png)

多长时间内做了多少笔操作就会进行拍照

![image-20240324201018297](/Users/chenxi/Library/Application Support/typora-user-images/image-20240324201018297.png)

多久多大进行一次rewrite

# Redis的过期策略 

### 1惰性删除（Lazy expiration） 

●当客户端尝试访问某个键时，Redis会先检查该键是否设置了过期时间，并判断是否过期。

●如果键已过期，则Redis会立即将其删除。这就是惰性删除策略。

该策略可以最大化地节省CPU资源，却对内存非常不友好。极端情况可能出现大量的过期key没有再次被访问，从而不会被清除，占用大量内存。

### 2定期删除（Active expiration） 

●Redis会每隔一段时间（默认100毫秒）随机检查一部分设置了过期时间的键。

●定期过期策略通过使用循环遍历方式，逐个检查键是否过期，并删除已过期的键值对。

通过调整定时扫描的时间间隔和每次扫描的限定耗时，可以在不同情况下使得CPU和内存资源达到最优的平衡效果

### Redis中同时使用了惰性过期和定期过期两种过期策略。 

●假设Redis当前存放20万个key，并且都设置了过期时间，如果你每隔100ms就去检查这全部的key，CPU负载会特别高，最后可能会挂掉。

●因此redis采取的是定期过期，每隔100ms就随机抽取一定数量的key来检查和删除的。

●但是呢，最后可能会有很多已经过期的key没被删除。这时候，redis采用惰性删除。在你获取某个key的时候，redis会检查一下，这个key如果设置了过期时间并且已经过期了，此时就会删除。

需要注意如果定期删除漏掉了很多过期的key，然后也没走惰性删除。就会有很多过期key积在内存中，可能会导致内存溢出，或者是业务量太大，内存不够用然后溢出了，为了应对这个问题，Redis引入了内存淘汰策略进行优化。

# Redis的淘汰策略

内存淘汰策略允许Redis在内存资源紧张时，根据一定的策略主动删除一些键值对，以释放内存空间并保持系统的稳定性。

### noeviction（不淘汰策略） 

当内存不足以容纳新写入数据时，Redis 将新写入的命令返回错误。这个策略确保数据的完整性，但会导致写入操作失败。

### volatile-lru（最近最少使用） 

从设置了过期时间的键中选择最少使用的键进行删除。该策略优先删除最久未被访问的键，保留最常用的键。

### volatile-ttl（根据过期时间优先） 

从设置了过期时间的键中选择剩余时间最短的键进行删除。该策略优先删除剩余时间较短的键，以尽量保留剩余时间更长的键。

### volatile-random（随机删除） 

从设置了过期时间的键中随机选择一个键进行删除。

### allkeys-lru（全局最近最少使用） 

从所有键中选择最少使用的键进行删除。无论键是否设置了过期时间，都将参与淘汰。

### 6allkeys-random（全局随机删除） 

从所有键中随机选择一个键进行删除。

# Redis基本使用

![image-20240402184349952](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402184349952.png)
![image-20240402184413286](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402184413286.png)
![image-20240402184903165](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402184903165.png)
![image-20240402184928081](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402184928081.png)
![image-20240402185139530](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402185139530.png)
![image-20240402185151528](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402185151528.png)

![image-20240402214930991](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402214930991.png)

![image-20240402220208453](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402220208453.png)

![image-20240402220238034](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402220238034.png)

![image-20240402223526409](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402223526409.png)

![image-20240402224807651](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402224807651.png)

![image-20240402224825730](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402224825730.png)

![image-20240402224856788](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402224856788.png)

![image-20240402224938529](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402224938529.png)

![image-20240402224959215](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402224959215.png)

![image-20240402225056826](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402225056826.png)

![image-20240402225209302](/Users/chenxi/Library/Application Support/typora-user-images/image-20240402225209302.png)

## Redis与数据库一致性

canal  监听binlog

![image-20240408153604047](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408153604047.png)
先删除缓存, 下次查缓存没有就去查数据库, 数据库查出来再同步到redis

![image-20240408154300759](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408154300759.png)
线程1执行更新操作, 删除redis缓存, 但是网络延迟了, 数据库还没更新, 是老的数据
线程2查redis, 由于已经删除了, 去查数据库, 是没更新的数据, 是老的, 所以数据库数据同步到redis了, 导致后面查到的都是老数据库

解决, 双删策略, 数据库更新时再删一次redis, 第二次删除要延迟删除

![image-20240408155847655](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408155847655.png)
先操作数据库, 线程1将数据库更新, 然后再删除缓存
线程2去redis查, 发现没有, 就会去数据库查, 查到就会同步到redis, 但是删除缓存可能出现问题, 删除失败问题

![image-20240408160106996](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408160106996.png)
代码耦合度过高, 要进行解耦
![image-20240408160242706](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408160242706.png)
![image-20240408160627945](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408160627945.png)
线程1删除缓存, 然后更新数据库, 但是网络延迟了, 数据库没更新, 线程2读取的时候, redis被删除了, 去数据库查, 只能查到老数据, 然后同步redis
数据库更新之后, 订阅binlog日志, 到cannel客户端, cannel客户端就会延迟删除redis, 删除失败就会异步发送需要删除的key到mq进行删除,发送成功到cannel客户端, 客户端再进行延迟删除

![image-20240408161229017](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408161229017.png)
线操作缓存还是数据库? 推荐先操作数据库

在什么场景 解决了什么问题 打到了什么效果
![image-20240408162620401](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408162620401.png)


![image-20240408162934969](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408162934969.png)
![image-20240408163012843](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163012843.png)
![image-20240408163028456](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163028456.png)
![](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163217791.png)

![image-20240408163059345](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163059345.png)

![image-20240408163354004](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163354004.png)

![image-20240408163429141](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408163429141.png)

![image-20240408164609629](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408164609629.png)

![image-20240408165119894](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408165119894.png)

![image-20240408165158566](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408165158566.png)

![image-20240408165219363](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408165219363.png)

批量发送到方式,效率高

## String

可以用来计数文章浏览量

## list

可以用来

## hash

可以用来存表

## Redis之set

**redis的set我更想管他叫做集合，里面数据不重复，我们可以比较两个集合的交集，并集，以及不同的地方，我们共同的关注人/联系人是不是就可以通过这个set来操作。**

set值是不能重复的，这是他的一个数据结构特性，在redis中也把set当做自己的一个基础数据类型。

```
Redis的Set是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。
```

![image-20240417074601054](/Users/chenxi/Library/Application Support/typora-user-images/image-20240417074601054.png)

可以用用来抽奖

## zset

朋友圈点赞

## 连接Redis

```java
/usr/local/bin/redis-cli  连接redis

shutdown 

redis-server /etc/redis.conf  启动redis

systemctl status firewalld   查看防火墙

systemctl stop firewalld  关闭防火墙

ps -ef | grep redis   查看进程
```

# Redis/Zookeeper/MySQL实现分布式锁

![image-20240410002842793](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410002842793.png)

redis乐观锁
![image-20240415231140997](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415231140997.png)
![image-20240410014559480](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410014559480.png)

new SessionCallback	![image-20240410014747916](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410014747916.png)
执行失败进行重试

## redis分布式锁

![image-20240415232859231](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415232859231.png)

#### 独占排他锁

![image-20240415233546457](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415233546457.png)
![image-20240415233256997](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415233256997.png)



setnx,设置了值,别的就不能设置了,所以设置了值就相当于获取锁了

![image-20240410015628788](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410015628788.png)

![image-20240410020917148](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410020917148.png)

![image-20240410020952683](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410020952683.png)
![image-20240415234709032](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415234709032.png)
1.2.3.4集群,1服务器宕机了, 就会出现死锁,所以要添加过期时间

![image-20240410021551270](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410021551270.png)
不行, 如果还没来得及执行就宕机了

防止误删,先判断是否是自己的锁,再进行删除,但是判断和删除不是原子性的,可能刚刚判断完就到期了,导致误删

![image-20240410022740403](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410022740403.png)

![image-20240416000047726](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416000047726.png)

![image-20240410154642195](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410154642195.png)

![image-20240410154727211](/Users/chenxi/Library/Application Support/typora-user-images/image-20240410154727211.png)

![image-20240416000609402](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416000609402.png)

![image-20240416000728768](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416000728768.png)
使用lua脚本设置防误删的原子性问题

## redisson分布式锁

![image-20240416002204421](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416002204421.png)

![image-20240416002318153](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416002318153.png)

![image-20240416002451951](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416002451951.png)

![image-20240416002618784](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416002618784.png)

## zookeeper分布式锁

![image-20240416002859987](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416002859987.png)

## 可重入锁reentrantLock

锁重入是说当前线程拿到锁了，然后后面又请求同一个锁，所以就会对state++，然后释放锁的时候state--，直到为0，再唤醒下一个等待的线程

可重入锁：可重入就是说某个线程已经获得某个锁,可以再次获取锁而不会出现死锁

## 什么是乐观锁和悲观锁?

**悲观锁**: 总是假设最坏的情况 ， 每次去拿数据的时候都认为别人会修改 ， 所以每次在拿数据的时候都会上锁 ， 这样别人想拿这个数据就会阻塞直到它拿到锁。传统的关系型数 据库里边就用到了很 多这种锁机制 ， 比如行锁 ， 表锁等 ， 读锁 ， 写锁等 ， 都是在做操 作之前先上锁。再比如 Java 里面的同步原语 synchronized 关键字的实现也是悲观锁。

**乐观锁** :顾名思义 ， 就是很乐观 ， 每次去拿数据的时候都认为别人不会修改 ，所以不会上锁 ， 但是在更新的时候会判断一下在此期间别人有没有去更新这个数据 ，可以使用版本号等机制。乐观锁 适用于多读的应用类型 ，这样可以提高吞吐量 ，像数据库提供的类似于write_condition 机制 ，其实都是提供的乐观锁。在 Java 中 java.util.concurrent.atomic 包下面的原子变量类就是使用了乐观锁的一种实现方式 CAS 实现的。

**乐观锁的实现方式**: 1、使用版本标识来确定读到的数据与提交时的数据是否一致。提交后修改版本标 识 ，不一致时可 以采取丢弃和再次尝试的策略。
 2、java 中的 CompareandSwap 即 CAS ，当 多个 线程 尝试 使用 CAS 同时 更新同 一 个 变 量时 ，只 有其 中 一 个线 程能 更新 变量 的值 ，而 其他 线程 都失 败 ，失败 的线程 并不 会被挂起，而是被告知这次竞争中失败，并可以再次尝试。 CAS操作中包含三个操作数 — — 需要 读写 的内 存位 置( V )、 进行 比较 的预 期原 值( A ) 和拟 写入 的 新 值(B) 。如果 内存 位置 V 的值 与预 期 原 值 A 相匹 配，那么 处 理 器会 自动将 该位 置 值 更新 为新 值 B 。 否 则处 理器 不做 任何 操作 。

**CAS 缺点**:
 1、**ABA 问题**:
 比如说一个线程 one 从内存位置 V 中取出 A ，这时候另一个线程 two 也从内存中 取出 A ， 并且 two 进行了一 些操作变成了 B ，然后 two 又将 V 位置的数据变成 A ， 这时候线程 one 进行 CAS 操作发现内存中仍然是 A ，然后 one 操作成功 。尽管线 程 one 的 CAS 操作成功， 但可能存在潜藏的问题。从 Java1. 5 开始 JDK 的 atomic 包里提供了 一 个类
 AtomicStamped Reference 来解决 ABA 问题。
 2、**循环时间长开销大**:
 对于资源竞争严重( 线程冲突严重) 的情况 ， CAS 自旋的概率会比较大 ， 从而浪费更 多的 CPU 资源 ，效率低于 synchronized。
 3、**只能保证一个共享变量的原子操作**:
 当对一个共享变量执行操作时 ，我们可以使用循环 CAS 的方式来保证原子操作 ，但是对多个共享变 量操作时 ，循环 CAS 就无法保证操作的原子性 ， 这个时候就可以用锁。









qps 多少是在哪儿看的

## 布隆过滤器是如何实现的

是二进制0和1表示存不存在的问题

假如有一个值, 10传入, 会有三个hash函数进行运算,分别会有一个值比如1 3 5 , 会把数组指针为1 3 5的值改为1
很难进行删除操作
误判:是指数据不存在布隆过滤器中,但是经过计算之后,发现是存在于布隆过滤器当中的![image-20240416040220638](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416040220638.png)
![image-20240416040313417](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416040313417.png)
![image-20240416040731487](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416040731487.png)
![image-20240416040827298](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416040827298.png)

# 
