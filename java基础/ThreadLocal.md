[toc]

# ThreadLocal有哪些应用场景？它底层是如何实现的？

## 1、是什么

是线程本地存储机制，将数据缓存在某个线程内部，该线程可以在任何时刻，任何方法中获取缓存的数据 ThreadLocalMap专门存放threadlocal的数据，当前线程中，将threadlocal对象存入ThreadLocalMap中的entry对象中的key中，将set的值放入value中

![image-20240422022007979](img/image-20240422022007979.png)

## 2、底层

是通过ThreadLocalMap来实现的，每个Thread线程对象中都存储着一个ThreadLocalMap

![image-20240422022044280](img/image-20240422022044280.png)