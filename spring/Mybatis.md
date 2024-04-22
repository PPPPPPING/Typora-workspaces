# Mybatis缓存机制



Mybatis 里面设计的二级缓存是用来提升数据的检索效率，避免每次数据的访问都需要去查 询数据库。 一级缓存，是 SqlSession 级别的缓存，也叫本地缓存，因为每个用户在执行查询的时候都需要使用 SqlSession 来执行，
为了避免每次都去查数据库，Mybatis 把查询出来的数据保存到 SqlSession 的本地缓存中，后续的 SQL 如果命中缓存，就可以直接从本地缓存读取了

如果想要实现跨 SqlSession 级别的缓存?那么一级缓存就无法实现了，因此在 Mybatis 里面引入了二 级缓存，就是当多个用户

在查询数据的时候，只要有任何一个 SqlSession 拿到了数据就会放入到二级缓存里面，其他的 SqlSession 就可以从二级缓存加载数据。

(如图)一级缓存的具体实现原理是: 在 SqlSession 里面持有一个 Executor，每个 Executor 中有一个 LocalCache 对象。 当用户发起查询的时候，Mybatis 会根据执行语句在 Local Cache 里面查询，如果没命中，再去查询 数据库并写入到 LocalCache，否则直接返回。 

所以，以及缓存的生命周期是 SqlSessiion，而且在多个 Sqlsession 或者分布式环境下，可能会导致数据库写操作出现脏数据。

(如图)二级缓存的具体实现原理是: 使用 CachingExecutor 装饰了 Executor，所以在进入一级缓存的查询流程之前，会先通过 CachingExecutor 进行二级缓存的查询。