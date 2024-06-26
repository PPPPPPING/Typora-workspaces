[toc]



# CAS

当且仅当旧的预期值A和内存V想同时,将内存值V修改为B否则什么都不做或重来

==当它重来重试这种行为称之为------自旋！==

![image-20240425000945573](img/image-20240425000945573.png)

![image-20240425001936752](img/image-20240425001936752.png)

![image-20240425001355813](img/image-20240425001355813.png)
Unsafe里面大多数都是native,直接操作操作系统的

![image-20240425002118549](img/image-20240425002118549.png)

![image-20240425002207731](img/image-20240425002207731.png)

![image-20240425002220122](img/image-20240425002220122.png)

![image-20240425002239646](img/image-20240425002239646.png)

![image-20240425002802059](img/image-20240425002802059.png)

![image-20240425002725591](img/image-20240425002725591.png)

```java
public class AtomicReferenceDemo {
    public static void main(String[] args) {
        AtomicReference<User> atomicReference = new AtomicReference<>();

        User z3 = new User("z3", 22);
        User li4 = new User("li4", 28);

        atomicReference.set(z3);

        System.out.println(atomicReference.compareAndSet(z3, li4) + "\t" + atomicReference.get().toString());
        System.out.println(atomicReference.compareAndSet(z3, li4) + "\t" + atomicReference.get().toString());


    }
}
```

## 自旋锁

![image-20240425003713941](img/image-20240425003713941.png)

## CAS两大问题

### 循环时间过长开销过大

### 解决ABA的问题用哪个类

![image-20240425004329874](img/image-20240425004329874.png)

解决ABA问题用AtomicStampedReference<Book> stampedReference类

![image-20240425010607317](img/image-20240425010607317.png)

## 总结

CAS是比较并且交换,是unsafe类下面的原子操作,unsafe大多都是native修饰的,而native底层是直接操作内存的,是一个操作系统语句,所以CAS是原子操作,他能实现自旋,自旋是指不停的重试修改的过程

场景落地:

- automatic原子类就是通过cas+volatile实现的,避免synchroniced的高开销
- ReentrantLock,底层就是通过自旋来获取和释放锁
- AQS也是通过自旋来获取和释放锁的

问题:

- 循环时间过长开销过大

- 会产生ABA问题,就是两个线程,A将a改成b之前,B已经将b改成a了,所以A还是a没有改成b
  用AtomicStampedReference可以解决问题，版本号时间戳，比较加版本号一起上

# volatile

## 可见性

写完后立即刷新回主内存并及时发出通过,大家可以去主内存拿最新版本,前面的修改对后面所有内存可见

## 禁止指令重排

![image-20240425015124212](img/image-20240425015124212.png)

## ==内存屏障(memory barrier)==

### 是什么

![image-20240425020006176](img/image-20240425020006176.png)

![image-20240425021531454](img/image-20240425021531454.png)

### 可见性如何保证

内存屏障之前的写操作都回写到主内存

内存屏障之后的读操作都能获取最新结果

### 禁止指令重排如何保证

![image-20240425022011036](img/image-20240425022011036.png)

是CPU货编译器对内存随机访问的一个同步点,使得此点之前到所有读写操作执行完之后才可以开始该同步点读操作

### 为什么不保证原子性

![image-20240425024206947](img/image-20240425024206947.png)

两个线程同时将0加1操作,但是A线程改了,B还没改完,主内存就会将值改为1,但是B内存就会失效,所以volatile不能保证原子性,对于多线程修改主内存共享变量的场景只能加==同步锁==

### 怎么实现的

### 场景落地

volatile boolean flag = false

高并发里面,靠变量来通知其他线程来改变后续动作

![image-20240425030549755](img/image-20240425030549755.png)

![image-20240425030231625](img/image-20240425030231625.png)

单例

![image-20240425030858458](img/image-20240425030858458.png)

![image-20240425031048498](img/image-20240425031048498.png)

问题: 多线程环境下,由于重排序,该对象可能还未完成初始化就被其他线程读取

解决: 利用volatile,禁止初始化对象和设置singleton指向内存空间重排序

## 总结

具有可见性:写完后立即刷新回主内存并及时发出通过,大家可以去主内存拿最新版本,前面的修改对后面所有内存可见

禁止指令重排序:有时操作系统会对代码指令进行重排序,而加了volatile就会禁止指令重排序

原理:内存屏障,是CPU或编译器在对内存随机访问的操作中的一个同步点，使得此点
之前的所有读写操作都执行后才可以开始执行此点之后的操作

是非原子性的:会造成写丢失

场景落地:

- volatile boolean flag = false,高并发里面,靠变量来通知其他线程来改变后续动作
- 双重检查锁
- volatile+cas可以实现原子类

# 举个volatile关键字例子

# 有用过future类吗

## 你觉得线程池中用这个Future类是解决了什么问题

# AQS

## 是什么

抽象的队列同步器

整体就是一个抽象的FIFO队列来完成资源获取线程的排队工作，并通过一个int类变量
表示持有锁的状态

和AQS有关的

ReentrantLock
CountDownLatch
ReentrantReadWriteLock
Semaphore

![image-20240425145427412](img/image-20240425145427412.png)

既然说到了排队等候机制，那么就一定会有某种队列形成，这样的队列是什么数据结构呢？

如果共享资源被占用，就需要一定的阻塞等待唤醒机制来保证锁分配。这个机制主要用的是CLH队列的变体实现的，将暂时获取不到锁的线程加入到队列中，这个队列就是AQS同步队列的抽象表现。它将要请求共享资源的线程及自身的等待状态封装成队列的结点对象(Node),通过CAS、自旋以LockSupport.park()的方式，维护state变量的状态，使并发达到同步的效果。

![image-20240425150923552](img/image-20240425150923552.png)

AQS使用一个volatile的int类型的成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取的排队工作将每条要去抢占资源的线程封装成一个Node节点来实现锁的分配，通过CAS完成对State值的修改。

![image-20240425151153850](img/image-20240425151153850.png)

![image-20240425151218521](img/image-20240425151218521.png)

![image-20240425151550142](img/image-20240425151550142.png)

属性说明

![image-20240425152503654](img/image-20240425152503654.png)

## 底层原理

Lock lock = new ReentrantLock();

不传就是默认非公平锁

Lock lock = new ReentrantLock(true);

传了true就是公平锁

ReentrantLock具体实现

![image-20240425153137012](img/image-20240425153137012.png)

![image-20240425153621256](img/image-20240425153621256.png)

![image-20240425154947548](img/image-20240425154947548.png)

## 总结

AQS是抽象的队列同步器,用于解决锁分配给”谁“的问题,整体就是一个抽象的FIFO队列来完成排队工作，并通过一个int类变量state表示持有锁的状态,根据排队等候机制,对暂时没抢到锁的线程让他去排队,抢到资源的线程就执行任务

排队等候机制:它将要请求共享资源的线程及自身的等待状态封装成队列的节点对象（Node），通过CAS自旋以及LockSupport.park()的方式，维护着state变量的状态，使其达到同步的状态。

原理,ReentrantLock为例:

分为公平锁和非公平锁

唯一的区别是公平锁多了一个限制条件：hasQueuedPredecessors()-----公平锁加锁时判断等待队列中是否存在有效节点的方法

都去实现acquire()方法,acquire又有三个核心方法tryAcquire()为获取锁,addWriter()为加入队列

最后tryRelease()方法，释放锁

# synchronized

进程：在系统中运行的一个应用程序，每个进程都有它自己的内存空间和系统资源
线程：也被称为轻量级进程，在同一个进程内会有1个或多个线程，是大多数操作系统进行时序调度的基本单元。
管程：Monitor（锁），也就是我们平时所说的锁。Monitor其实是一种同步机制，它的义务是保证（同一时间）只有一个线程可以访问被保护的数据和代码，JVM中同步是基于进入和退出监视器（Monitor管程对象）来实现的，每个对象实例都会有一个Monitor对象，Monitor对象和Java对象一同创建并销毁，底层由C++语言实现。

1.为什么每个对象都能成为一把锁

Monitori可以理解为一种同步工具，也可理解为一种同步机制，常常被描述为一个Java对象。Java对象是天生的Monitor,每一个Java对象都有成为Monitor的潜质，因为在Java的设计中，每一个Java对象自打娘胎里出来就带了一把看不见的锁，它叫做内部锁或者Monitor锁。

![image-20240428163430218](img/image-20240428163430218.png)

![image-20240428163513089](img/image-20240428163513089.png)

Monitor的本质是依赖于底层操作系统的Mutex Lock实现，操作系统实现线程之间的切换需要从用户态到内核态的转换，成本非常高。

JVM中的同步就是基于进入和退出管程(Monitor).对象实现的。每个对象实例都会有一个Monitor,Monitor可以和对象一起创建、销毁。
Monitor是由ObjectMonitor3实现，而ObjectMonitor是由C++的ObjectMonitor..hpp文件实现，如下所示：

## 无锁

## 偏向锁

偏向锁:单线程竞争
当线程A第一次竞争到锁时，通过操作修改MarkWord中的偏向线程ID、偏向模式如果不存在其他线程竞争，那么持有偏向锁的线程将永远不需要进行同步。

![image-20240428215832804](img/image-20240428215832804.png)

实际上编向锁在JDK1.6之后是默认开启的，但是启动时间有延迟，所以需要添加参数-XX:BiasedLockingStartupDelay=0，让其在程序启动时立刻启动
开启偏向锁:
-XX:+UseBiasedLocking -Xx:BiasedLockingstartupDelay=0
关闭偏向锁:关闭之后程序默认会直接进入-----XX:-UseBiasedLocking
轻量级锁状态。

## 总结

是同步锁,在不同场景下对锁进行升级完成不同的锁功能,他有无锁,偏向锁,轻量锁,重量锁,锁升级功能主要依赖Java对象头里的MarkWord中锁标志位和释放偏向锁标志位

偏向锁：MarkWord存储的是偏向的线程ID
轻量锁：MarkWord存储的是指向线程栈中Lock Record的指针
重量锁：MarkWord存储的是指向堆中的monitor对象（系统互斥量指针）

无锁,创建一个对象没有任何线程进行竞争,就是无锁

偏向锁,单线程竞争,一段同步代码被一个线程多次访问,后续访问就自动获取锁就叫偏向锁

轻量锁,CAS自旋排队获取锁

![image-20240430022933336](img/image-20240430022933336.png)

重量锁

自旋到达一定的次数没有成功就变成重量级锁

![img](img/1681455335764-3bb5bb5f-d014-4b25-9755-e0ca2c9aa809.png)

![image-20240430023438607](img/image-20240430023438607.png)

![20200602120540100.jpg](img/1681459024640-cadca197-5d19-433d-b6d8-abc8965ce494.jpeg)



# 20 锁用过哪些

# 21 锁升级

# 22 锁降级

# 23 juc包下的用过哪些

# 24 分段锁的原理

# 25 自旋中的ABA问题

# Threadlocal底层的原理锁升级

## 是什么(说一下大概概念)

![image-20240425031712554](img/image-20240425031712554.png)

![image-20240425034408013](img/image-20240425034408013.png)

## 底层原理实现

![image-20240425040110273](img/image-20240425040110273.png)

<img src="img/image-20240425040414413.png" alt="image-20240425040414413" style="zoom:33%;" />

## 内存泄漏

什么是内存泄漏?

如何引起的?

### 为什么要用弱引用?

![image-20240425043827224](img/image-20240425043827224.png)

![image-20240425044020851](img/image-20240425044020851.png)

![image-20240425044632065](img/image-20240425044632065.png)

## 强软弱虚引用

![image-20240425040919249](img/image-20240425040919249.png)

### 强引用

![image-20240425041920889](img/image-20240425041920889.png)

### 软引用

![image-20240425043341964](img/image-20240425043341964.png)

### 弱引用

![image-20240425043418349](img/image-20240425043418349.png)

### 虚引用

![image-20240425043450651](img/image-20240425043450651.png)

## 场景落地

```java
ThreadLocal<Integer> saleVolume = ThreadLocal.withInitial(() -> 0);

public void saleVolumeByThreadLocal() {
    saleVolume.set(1 + saleVolume.get());
}
```

```java
class House { //资源类
    int saleCount = 0;

    public synchronized void saleHouse() {
        ++saleCount;
    }

    /*ThreadLocal<Integer> saleVolume = new ThreadLocal<Integer>(){
        @Override
        protected Integer initialValue()
        {
            return 0;
        }
    };*/
    ThreadLocal<Integer> saleVolume = ThreadLocal.withInitial(() -> 0);

    public void saleVolumeByThreadLocal() {
        saleVolume.set(1 + saleVolume.get());
    }
}

/**
 * @auther zzyy
 * @create 2021-12-31 15:46
 * <p>
 * 需求1： 5个销售卖房子，集团高层只关心销售总量的准确统计数。
 * <p>
 * 需求2： 5个销售卖完随机数房子，各自独立销售额度，自己业绩按提成走，分灶吃饭，各个销售自己动手，丰衣足食
 */
public class ThreadLocalDemo {
    public static void main(String[] args) throws InterruptedException {

        House house = new House();

        for (int i = 1; i <= 5; i++) {
            new Thread(() -> {
                int size = new Random().nextInt(5) + 1;
                try {
                    for (int j = 1; j <= size; j++) {
                        house.saleHouse();
                        house.saleVolumeByThreadLocal();
                    }
                    System.out.println(Thread.currentThread().getName() + "\t" + "号销售卖出：" + house.saleVolume.get());
                } finally {
                    house.saleVolume.remove();
                }
            }, String.valueOf(i)).start();
        }
        ;

        //暂停毫秒
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(Thread.currentThread().getName() + "\t" + "共计卖出多少套： " + house.saleCount);
    }
}
```



# Synchronized和Lock的区别

从功能角度来看，Lock 和 Synchronized 都是 Java 中用来解决线程安全问题的工具。

从特性来看

Synchronized 是 Java 中的同步关键字，Lock 是 J.U.C 包中提供的接口，这个接口有很多实 现类，其中就包括 ReentrantLock 重入锁

Synchronized 可以通过两种方式来控制锁的粒度，

一种是把 synchronized 关键字修饰在方法层面，
 另一种是修饰在代码块上，并且我们可以通过 Synchronized 加锁对象的生命周期来控制锁的作用范 围，比如锁对象是静态对象或者类对象，那么这个锁就是全局锁。 如果锁对象是普通实例对象，那这个锁的范围取决于这个实例的生命周期。
 Lock 锁的粒度是通过它里面提供的 lock()和 unlock()方法决定的(贴图)，包裹在这两个方法之间 的代码能够保证线程安全性。而锁的作用域取决于 Lock 实例的生命周期。

![image-20240423145102956](img/image-20240423145102956.png)

Lock 比 Synchronized 的灵活性更高，Lock 可以自主决定什么时候加锁，什么时候释放锁， 只需要调用 lock()和 unlock()这两个方法就行，同时 Lock 还提供了非阻塞的竞争锁方法 tryLock()方法，这个方法通过返回 true/false 来告诉当前线程是否已经有其他线程正在使用 锁。

Synchronized 由于是关键字，所以它无法实现非阻塞竞争锁的方法，另外，Synchronized 锁的释放 是被动的，就是当 Synchronized 同步代码块执行完以后或者代码出现异常时才会释放。

 Lock 提供了公平锁和非公平锁的机制，公平锁是指线程竞争锁资源时，如果已经有其他线程 正在排队等待锁释放，那么当前竞争锁资源的线程无法插队。而非公平锁，就是不管是否有线 程在排队等待锁，它都会尝试去竞争一次锁。 Synchronized 只提供了一种非公平锁的实现。

从性能方面来看，Synchronized 和 Lock 在性能方面相差不大，在实现上会有一些区别， Synchronized 引入了偏向锁、轻量级锁、重量级锁以及锁升级的方式来优化加锁的性能，而 Lock 中则用到了自旋锁的方式来实现性能优化。

功能层面有两类锁

# 共享锁

# 排它锁

<img src="img/image-20240423223313062.png" alt="image-20240423223313062" style="zoom:50%;" />

<img src="img/image-20240423222948700.png" alt="image-20240423222948700" style="zoom:50%;" />

![image-20240423223334057](img/image-20240423223334057.png)

# JUC

JUC：指的是java.util三个并发编程工具包

1. java.util.concurrent
2. java.util.concurrent.atomic
3. java.util.concurrent.locks
