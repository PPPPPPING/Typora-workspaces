# Java基础

## 线程池

![image-20240416013707375](https://github.com/PPPPPPING/Typora-workspaces@img/img/image-20240416013707375.png)

### 线程池中核心线程数量大小怎么设置

CPU密集型任务：比如像加解密，压缩、计算等一系列需要大量耗费 CPU 资源的任务，大部分场景下都是纯 CPU 计算。尽量使用较小的线程池，一般为CPU核心数+1。因为CPU密集型任务使得CPU使用率很高，若开过多的线程数，会造成CPU过度切换。
IO密集型任务：比如像 MySQL 数据库、文件的读写、网络通信等任务，这类任务不会特别消耗 CPU 资源，但是 IO 操作比较耗时，会占用比较多时间。可以使用稍大的线程池，一般为2*CPU核心数。IO密集型任务CPU使用率并不高，因此可以让CPU在等待IO的时候有其他线程去处理别的任务，充分利用CPU时间。
另外：线程的平均工作时间所占比例越高，就需要越少的线程；线程的平均等待时间所占比例越高，就需要越多的线程；
以上只是理论值，实际项目中建议在本地或者测试环境进行多次调优，找到相对理想的值大小。

### 如何优化线程池的性能

要优化线程池的性能，需要根据实际情况进行参数配置。以下是一些优化建议：
1根据应用场景和任务性质，合理设置核心线程数（corePoolSize）和最大线程数（maximumPoolSize）。如果任务主要是IO密集型的，核心线程数可以设置为CPU核心数的两倍左右，最大线程数可以设置为CPU核心数的四倍左右。如果任务是CPU密集型的，核心线程数可以适当减少，最大线程数也可以适当减少。
2根据任务性质和实际需求，选择合适的任务队列（workQueue）。例如，如果任务是CPU密集型的，可以选择容量较大的有界队列，以减少线程的创建和销毁；如果任务是I/O密集型的，可以选择容量较小的无界队列，以避免队列过小导致的任务拒绝问题。
3根据系统资源和任务性质，合理设置线程的存活时间（keepAliveTime）。如果系统资源充足且任务性质不紧张，可以适当增加线程存活时间，以减少线程的创建和销毁；如果系统资源有限或任务性质较为紧张，可以适当减少线程存活时间，以减少线程的空闲时间。
4根据实际需求，自定义线程工厂（threadFactory）和任务拒绝策略（handler）。可以通过实现自己的线程工厂来设置线程的名称、优先级等属性，以提高线程池的可维护性。当任务队列已满且线程数达到最大值时，可以使用任务拒绝策略来处理无法执行的任务，例如抛出异常、记录日志或尝试重新提交等。
总之，在优化线程池性能时，需要根据实际情况进行参数配置，并选择合适的队列类型和任务拒绝策略。同时，还需要注意系统资源的利用和线程池的可维护性。

## 多线程

### 线程是什么  

线程是程序中执行的线程，Java虚拟机允许程序同时运行多个执行线程

### 如何避免死锁问题

### 线程安全问题

- #### 临界资源问题

  多个线程同时访问相同的资源并进行读写操作
  解决思路：避免竞态条件，多线程同步，必须获得每一个线程对象的锁（lock)
  
- #### 死锁

  死锁也是一种因为对资源争夺而出现的状态，是指两个或两个以上的进程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们将一直互相等待而无法推进下去。不断地加锁,锁中锁,锁套锁,最后造成循环，就可能会成为死锁；因此要解决死锁，尽量避免对同一资源的剥夺,请求和保持，避免循环等待。

## 集合

### Collection接口

所有集合类都位于java.util包下。Java的集合类主要由两个接口派生而出：Collection和Map，Collection和Map是Java集合框架的根接口，这两个接口又包含了一些接口或实现类。

* Collection一次存一个元素，是单列集合；
* Map一次存一对元素，是双列集合。Map存储的一对元素：键--值，键（key）与值（value）间有对应（映射）关系

Collection集合主要有List和Set两大接口

### Collection的子接口之一List

```java
reverse(List):反转List中的顺序
shuffle(List)：对List集合元素进行随机排序
sort(List):根据元素的自然排序对指定List集合元素按照升序排序
sort(List,Conparator):根据指定的Comparator产生的顺序对List集合进行排序
swap(List,int,int):将指定list集合中的i处元素和j处元素进行调换
    
Object max(Collection):根据元素的自然顺序，返回给定集合中最大的元素
Object max(Collection,Comparator):根据Comparator指定的顺序，返回给定集合中的最大
Object min(Collecion)
Object min(Collection,Comparator)
int frequency(Collecion,Object):返回指定集合中指定元素出现的次数
void copy(List dest,List src):将src中的内容复制到dest中
boolean replaceAll(List list,Object oldVal,Object newVal):使用新值替换list对    
```

List是有序的，是可重复的，动态数组，一旦初始化了，长度就不能改变。
List的主要实现：ArrayList、LinkedList、Vetor
List的常用方法：
![List常用方法](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvTGlzdCVFNSVCOCVCOCVFNyU5NCVBOCVFNiU5NiVCOSVFNiVCMyU5NS5wbmc)

### ArrayList、LinkList和Vector三者的区别

|              |                 ArrayList                  |                          LinkedList                          |             Vector              |
| :----------: | :----------------------------------------: | :----------------------------------------------------------: | :-----------------------------: |
|   底层实现   |                  动态数组                  |                           双向链表                           |              数组               |
| 同步性几效率 |  不同步，非线程安全，效率高，支持随机访问  |                  不同步，非线程安全，效率高                  |     同步，线程安全，效率低      |
|     特点     | 开辟的内存空间是连续的，查询快，但是增删慢 | 开辟的内存空间是可连续的，也可不连续，节点与节点之间通过指针关联，当增删时不需要关心位置，这也为它增删效率奠定了基础。但是查询慢 |          查询快增删慢           |
|   默认容量   |                     10                     |                          无初始容量                          |               10                |
|   扩容机制   |     而后按照当前的容量的1.5倍进行扩容      |                          无扩容机制                          | 而后按照当前的容量的2倍进行扩容 |

都是List接口的实现类，存储数据的特点相同，都是有序，可重复。底层使用object[]存储。

- ArrayList：作为list接口的主要实现类，是线程不安全的，效率高。
- LinkList：对频繁的插入，删除操作，使用此类的效率必ArrayList高，底层使用的是双向链表。
- Vector：古老实现类，线程安全的，效率低。底层使用object[]存储。

### ArrayList和LinkedList的区别

可以从四个角度看待问题：数据结构、内存分配、默认长度、扩容机制。

- 使用场景ArrayList是动态数组，LinkList是双向列表。
- 数组：内存中开辟空间就相当一群人 站成一对，从0开始。
- 数组可分为索引数组和关联数组。
- ArrayList是索引数组中的动态数组。

####  Arrayist

1. 基于索引的动态数组，开辟的内存空间是连续的，这也为他查询效率奠定了基础。
2. 初始化时默认容量是10，而后按照当前的容量的1.5被进行扩容。
3. 适用在查找多，增删少的场景。

#### LinkedList

1. 双向链表，开辟的内存空间是可连续的，也可不连续，节点与节点之间通过指针关联，当增删时不需要关心位置，这也为它增删效率奠定了基础。
2. 由于是双向链表，所以无初始容量，无扩容机制。
3. 适用在增删多，查找少的场景。

### Collection的子接口之一Set

Set接口是无序的，不可重复的。
Set接口的主要实现类：HashSet、TreeSet
Set接口的常用方法：
![Set常用方法](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvU2V0JUU1JUI4JUI4JUU3JTk0JUE4JUU2JTk2JUI5JUU2JUIzJTk1LnBuZw)

### Set接口的框架

|            |                    **HashSet**                    |                         **TreeSet**                          |                      **LinkedHashSet**                       |
| :--------: | :-----------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  底层实现  |                      HashMap                      |                            红黑树                            |                        LinkedHashMap                         |
|   重复性   |                    不允许重复                     |                          不允许重复                          |                          不允许重复                          |
|   有无序   |                       无序                        | 有序，支持两种排序方式，自然排序和定制排序，其中自然排序为默认的排序方式。 |           有序，以元素插入的顺序来维护集合的链接表           |
| 时间复杂度 | add()，remove()，contains()方法的时间复杂度是O(1) |     add()，remove()，contains()方法的时间复杂度是O(logn)     | LinkedHashSet在迭代访问Set中的全部元素时，性能比HashSet好，但是插入时性能稍微逊色于HashSet，时间复杂度是 O(1)。 |
|   同步性   |                不同步，线程不安全                 |                      不同步，线程不安全                      |                      不同步，线程不安全                      |
|   null值   |                    允许null值                     | 不支持null值，会抛出 java.lang.NullPointerException 异常。因为TreeSet应用 compareTo() 方法于各个元素来比较他们，当比较null值时会抛出 NullPointerException异常。 |                          允许null值                          |
|    比较    |                     equals()                      |                         compareTo()                          |                           equals()                           |

collection接口；单列集合，用来存储一个一个的对象。

Set接口；存储无序的，不可重复的数据。

HashSet：作为Set接口的主要实现类，线程是不安全的，可以存储null值。

LinkedHashSet：作为HashSet的子类，遍历其内部数据时i，可以按照添加的。

TreeSet：可以按照添加对象的指定属性，进行排序。

Set接口中没有额外定义新的方法，使用的都是collection中声明的方法。

### Map接口

Map：双列数据，存储key-value对的数据。Map 是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。 Map没有继承于Collection接口，从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。
Map 的常用实现类：HashMap、TreeMap、HashTable、LinkedHashMap、ConcurrentHashMap

#### 双列集合继承关系图

![双列集合](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvbWFwLnBuZw)

#### Map的常用方法

![Map常用方法](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvTWFwJUU1JUI4JUI4JUU3JTk0JUE4JUU2JTk2JUI5JUU2JUIzJTk1LnBuZw)

HashMap：作为Map的主要实现类，线程是不安全的，效率高，存储null的key和value。

LinkedHashMap：保证在遍历map元素时，可以按照提那家店顺序实现遍历；原因：在原有的HashMap底层的结构基础上，添加了一对指针，指向前一个和后一个元素，对于频繁的遍历操作，子类执行效率高于HashMap。

TreeMap保证按照添加到key-value对进行排序，实现排序遍历，此时考虑key的自然排序或定制排序；底层时红黑树。

hashTable：作为古老的实现类。线程是安全的，但是效率很低，不能存储null的key和value。

properties：常用来处理配置文件，key和value都是String类型。

|          |                            HshMap                            |                   HashTable                   |                           TreeMap                            |
| :------: | :----------------------------------------------------------: | :-------------------------------------------: | :----------------------------------------------------------: |
| 底层实现 |                     哈希表（数组+链表）                      |              哈希表（数组+链表）              |                            红黑树                            |
|  同步性  |                          线程不同步                          |                     同步                      |                          线程不同步                          |
|  null值  | 允许 key 和 Vale 是 null，但是只允许一个 key 为 null，且这个元素存放在哈希表 0 角标位置 |           不允许key、value 是 null            | value允许为null。 当未实现 Comparator 接口时，key 不可以为null 当实现 Comparator 接口时，若未对 null 情况进行判断，则可能抛 NullPointerException 异常。如果针对null情况实现了，可以存入，但是却不能正常使用get()访问，只能通过遍历去访问。 |
|   hash   | 使用hash(Object key)扰动函数对 key 的 hashCode 进行扰动后作为 hash 值 | 直接使用 key 的 hashCode() 返回值作为 hash 值 |                                                              |
|   容量   |                 容量为 2^4 且容量一定是 2^n                  |          默认容量是11，不一定是 2^n           |                                                              |
|   扩容   |           两倍，且哈希桶的下标使用 &运算代替了取模           |       2倍+1，取哈希桶下标是直接用模运算       |                                                              |

#### HashMap的底层

数组+链表（jdk7以及以前）

数组+链表+红黑树（jdk8）

#### HashMap和HashSet的区别

HashSet的实现方式大致如下，通过一个HashMap存储元素，元素是存放在HashMap的Key中，而Value统一使用一个Object对象。

#### HashMap和TreeMap的区别

Map：在数组中是通过数组下标来对 其内容进行索引的，而Map是通过对象来对 对象进行索引的，用来 索引的对象叫键key，其对应的对象叫值value。

- HashMap是通过hashcode()对其内容进行快速查找的；HashMap中的元素是没有顺序的；TreeMap中所有的元素都是有某一固定顺序的，如果需要得到一个有序的结果，就应该使用TreeMap。
  HashMap和TreeMap都不是线程安全的。
- HashMap继承AbstractMap类；覆盖了hashcode() 和equals() 方法，以确保两个相等的映射返回相同的哈希值；TreeMap继承SortedMap类；他保持键的有序顺序；
- HashMap：基于hash表实现的；使用HashMap要求添加的键类明确定义了hashcode() 和equals() （可以重写该方法）；为了优化HashMap的空间使用，可以调优初始容量和负载因子；
- TreeMap：基于红黑树实现的；TreeMap就没有调优选项，因为红黑树总是处于平衡的
- HashMap：适用于Map插入，删除，定位元素。
- TreeMap：适用于按自然顺序或自定义顺序遍历键（key）。

### 集合工具类Collections

Collections：集合工具类，方便对集合的操作。这个类不需要创建对象，内部提供的都是静态方法。

#### Collection 和 Collections的区别

```xml
Collections是个java.util下的类，是针对集合类的一个工具类,提供一系列静态方法,实现对集合的查找、排序\替换、线程安全化（将非同步的集合转换成同步的）等操作。

Collection是个java.util下的接口，它是各种集合结构的父接口，继承于它的接口主要有Set和List,提供了关于集合的一些操作,如插入、删除、判断一个元素是否其成员、遍历等。
```

## Mybatis 里面的缓存机制

Mybatis 里面设计的二级缓存是用来提升数据的检索效率，避免每次数据的访问都需要去查 询数据库。
 一级缓存，是 SqlSession 级别的缓存，也叫本地缓存，因为每个用户在执行查询的时候都需要使用 SqlSession 来执行，

为了避免每次都去查数据库，Mybatis 把查询出来的数据保存到 SqlSession 的本地缓存中，后续的 SQL 如果命中缓存，就可以直接从本地缓存读取了

如果想要实现跨 SqlSession 级别的缓存?那么一级缓存就无法实现了，因此在 Mybatis 里面引入了二 级缓存，就是当多个用户

在查询数据的时候，只要有任何一个 SqlSession 拿到了数据就会放入到二级缓存里面，其他的 SqlSession 就可以从二级缓存加载数据。

 (如图)一级缓存的具体实现原理是:
 在 SqlSession 里面持有一个 Executor，每个 Executor 中有一个 LocalCache 对象。 当用户发起查询的时候，Mybatis 会根据执行语句在 Local Cache 里面查询，如果没命中，再去查询 数据库并写入到 LocalCache，否则直接返回。
 所以，以及缓存的生命周期是 SqlSessiion，而且在多个 Sqlsession 或者分布式环境下，可能会导致数据库写操作出现脏数据。

(如图)二级缓存的具体实现原理是:
 使用 CachingExecutor 装饰了 Executor，所以在进入一级缓存的查询流程之前，会先通过 CachingExecutor 进行二级缓存的查询。

## String

```java
package com.chenxi.huawei;

import org.junit.Test;

import java.lang.reflect.Parameter;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.*;

/**
 * @Author cxi
 * @Date 2022/7/11 18:51
 */
public class StringTest {
    public static void main(String[] args) {

    }

    @Test
    public void founction() {
        String str = "ChenxiWangying";
        //3.charAt(int index)从字符中取出指定索引的值
        System.out.println(str.charAt(3));
        //4.indexOf(String str)方法,查找对应字符在字符串中的索引位置，如果没有则返回-1，常与3配合使用，
        System.out.println(str.indexOf("i"));
        //lastIndexOf(String str)方法，查找对应字符最后在字符串中出现的索引位置，如果没有则返回-1
        System.out.println(str.lastIndexOf("i"));

        //toCharArray()方法，将字符串变成一个数组
        char[] charArray = str.toCharArray();
        for (char c : charArray) {
            System.out.print(c);
        }
        System.out.println(Arrays.toString(charArray));

        //toUpperCase()将字符串全部转换为大写
        String strBig = str.toUpperCase();
        System.out.println(strBig);

        //8.toLowerCase()将字符串全部转换为小写
        String strSmall = str.toLowerCase();
        System.out.println(strSmall);

        //split("字符")，根据给定的正则表达式来拆分字符串，形成一个String数组
        String[] strArray = str.split("");
        System.out.println(Arrays.toString(strArray));

        //trim()方法，去除字符串左右两端的空白，该方法只能去除左右，中间的没办法
        String strSpaces = "  Chenxi Wangying  ";
        System.out.println(strSpaces.trim());

        //11.substring(int beginIndex,int endIndex)截取字符串
        System.out.println(str.substring(1, 3));
        System.out.println(str.substring(4));

        //12.equalsIgnoreCase(String str),忽略字符串大小比较字符串的值，
        String strA = "absC";
        String strB = "absc";
        System.out.println(strA.equalsIgnoreCase(strB));

        // concat(String str),将str的字符串的内容添加到字符串的后面，效果等同于+
        System.out.println(str.concat("addTest"));

        //replace(char oldChar,char newChar),该方法用字符newChar替换掉当前字符串中所有的oldChar。
        System.out.println(str.replace("i", "???"));

        //replaceFirst(String regex,String replacement),该方法用字符replacement替换掉当前字符串中第一个匹配regex。
        System.out.println(str.replaceFirst("h", "???"));

        //17.startsWith(String prefix)，比较该字符串是否以prefix子字符串开始的
        System.out.println(str.startsWith("Chen"));
        System.out.println(str.startsWith("chen"));

        //endsWith(String prefix)，比较该字符串是否以prefix结尾的
        System.out.println(str.endsWith("ing"));

        //19.valueOf(Type type)用于将基本数据类型转换为String类型，补充一点，type不能为null，不然会报空指针异常
        String strType = String.valueOf(21);
        System.out.println(strType);

        //20.getBytes()，将该字符串转换为字节数组
        String strByte = "123421421";
        byte[] bytes = strByte.getBytes();
        System.out.println(Arrays.toString(bytes));

        //21.String.format()方法，字符串类型格式话
        //format(String format,Object obj)，新字符串使用本地语言环境，制定字符串格式和参数生成格式的新字符串
        //format(Locale locale,String format,Object obj),使用指定语言环境，制定字符串格式和参数生成格式的新字符串

        Date date = new Date();
        //c的使用
        System.out.printf("全部日期和时间信息：%tc%n", date);
        //f的使用
        System.out.printf("年-月-日格式：%tF%n", date);
        //d的使用
        System.out.printf("月/日/年格式：%tD%n", date);
        //r的使用
        System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n", date);
        //t的使用
        System.out.printf("HH:MM:SS格式（24时制）：%tT%n", date);
        //R的使用
        System.out.printf("HH:MM格式（24时制）：%tR", date);

    }

}
```

## 基本数据类型和包装类的区别

- 包装类是对象，拥有方法和字段，对象的调用都是通过引用对象的地址，基本类型不是。
- 包装类型是引用的传递，基本类型是值的传递。
- 声明方式不同，基本数据类型不需要new关键字，而包装类型需要new在堆内存中进行new来分配内存空间。
- 存储位置不同，基本数据类型直接将值保存在值栈中，而包装类型是把对象放在堆中，然后通过对象的引用来调用他们。
- 初始值不同，eg： int的初始值为 0 、 boolean的初始值为false 而包装类型的初始值为null。

## 冒泡排序

```java
package com.chenxi.listTestDemo.bubblesort;

/**
 * 冒泡排序
 */
public class BubbleSort {
    public static void main(String[] args) {
        int[] ints = new int[]{-10, -20, -30, -15, 10, 16, 18, 19, 60, 45, 88};
        for (int i = 0; i < ints.length - 1; i++) {
            for (int j = 0; j < ints.length - 1 - i; j++) {
                if (ints[j] > ints[j + 1]) {
                    int exchange = ints[j];
                    ints[j] = ints[j + 1];
                    ints[j + 1] = exchange;
                }

            }
        }
        for (int i = 0; i < ints.length; i++) {
            System.out.println(ints[i] + "\t");
        }
    }
}
```

```go
发生阿斯蒂芬撒旦法时间跨度发啦手机每当我

```

```python
asdf asdfjkalsdflasjfklasdfjlsajdfaslkdfjl kasdfj aslkdfjklasjdfklasjdfasdniasdfn 
```

## 重载与重写的区别

- 重写就是子类继承父类了，但是子类不想要父类那个方法，就可以将该方法重写
- 重载就是方法名相同，但是形参不同

## equals与==的区别

- ==比较的地址值
- equals比较的是内容

## Object类里面有哪些方法？

- ### getClass()

  Class<? extendsObject> getClass() 返回一个对象的运行时类。

- ### hashCode()

  int hashCode() 返回该对象的哈希码值。

- ### equals()

  boolean equals(Object obj) 指示某个其他对象是否与此对象“相等”。

- ### toString()

  String toString() 返回该对象的字符串表示。

- ### clone()

  protected Object clone() 创建并返回此对象的一个副本。

- ### wait()

  void wait() 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法。void wait(long timeout) 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或notifyAll() 方法，或者超过指定的时间量。
  void wait(long timeout, int nanos) 导致当前的线程等待，直到其他线程调用此对象的 notify()。

- ### notify()

  void notify() 唤醒在此对象监视器上等待的单个线程。

- ### notifyAll()

  void notifyAll() 唤醒在此对象监视器上等待的所有线程。

- ### finalize()

  protected void finalize() 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。

## Springbooot+Vue项目流程

首先建立一个前端Vue的项目。
然后建立一个maven项目，将网页的src和xml导入。
然后建立一个common包放配置类。
前端字段对应好之后。
utils > request.js 复制axios的代码。

```vue
request.post("/onepice",this.form).then(res => {
  console.log(res)
})

前面要加上端口：
request.post("localhost9090/onepice",this.form).then(res => {
  console.log(res)
})

跨域配置

创建vue.config.js
```

## String、StringBuffer和StringBuilder的异同？

相同点：底层都是通过char数组实现的
不同点：

- String对象一旦创建，其值是不能修改的，如果要修改，会重新开辟内存空间来存储修改之后的对象；而StringBuffer和StringBuilder对象的值是可以被修改的；
- StringBuffer几乎所有的方法都使用synchronized实现了同步，线程比较安全，在多线程系统中可以保证数据同步，但是效率比较低；而StringBuilder 没有实现同步，线程不安全，在多线程系统中不能使用 StringBuilder，但是效率比较高。
- 如果我们在实际开发过程中需要对字符串进行频繁的修改，不要使用String，否则会造成内存空间的浪费；当需要考虑线程安全的场景下使用 StringBuffer，如果不需要考虑线程安全，追求效率的场景下可以使用 StringBuilder。

## ThreadLocal有哪些应用场景？它底层是如何实现的？

1、是什么：是线程本地存储机制，将数据缓存在某个线程内部，该线程可以在任何时刻，任何方法中获取缓存的数据
ThreadLocalMap专门存放threadlocal的数据，当前线程中，将threadlocal对象存入ThreadLocalMap中的entry对象中的key中，将set的值放入value中

![image-20240416070530392](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416070530392.png)

2、底层是通过ThreadLocalMap来实现的，每个Thread线程对象中都存储着一个ThreadLocalMap

<img src="/Users/chenxi/Library/Application Support/typora-user-images/image-20240416070631328.png" alt="image-20240416070631328" style="zoom:50%;" />

## 设计模式

### Spring 框架中都用到了哪些设计模式？

简单工厂：
BeanFactory：Spring的BeanFactory充当工厂，负责根据配置信息创建Bean实例。它是一种工厂模式的应用，根据指定的类名或ID创建Bean对象。
工厂方法：
FactoryBean：FactoryBean接口允许用户自定义Bean的创建逻辑，实现了工厂方法模式。开发人员可以使用FactoryBean来创建复杂的Bean实例。
单例模式：
Bean实例：Spring默认将Bean配置为单例，确保在容器中只有一个共享的实例，这有助于节省资源和提高性能。
适配器模式：
SpringMVC中的HandlerAdapter：SpringMVC的HandlerAdapter允许不同类型的处理器适配到处理器接口，以实现统一的处理器调用。这是适配器模式的应用。
装饰器模式：
BeanWrapper：Spring的BeanWrapper允许在不修改原始Bean类的情况下添加额外的功能，这是装饰器模式的实际应用。
代理模式：
AOP底层：Spring的AOP（面向切面编程）底层通过代理模式来实现切面功能，包括JDK动态代理和CGLIB代理。
观察者模式：
Spring的事件监听：Spring的事件监听机制是观察者模式的应用，它允许组件监听和响应特定类型的事件，实现了松耦合的组件通信。
策略模式：
excludeFilters、includeFilters：Spring允许使用策略模式来定义包扫描时的过滤策略，如在@ComponentScan注解中使用的excludeFilters和includeFilters。
模板方法模式：
Spring几乎所有的外接扩展：Spring框架的许多模块和外部扩展都采用模板方法模式，例如JdbcTemplate、HibernateTemplate等。
责任链模式：
AOP的方法调用：Spring AOP通过责任链模式实现通知（Advice）的调用，确保通知按顺序执行。

### 单例模式

```java
// 单例模式
class Singleton {
    private static Singleton singleton;
    private Singleton(){};

    private static Singleton singleton() {
        return singleton;
    }
}

// 饿汉式 先创建实例再用
class HungrySingleton {
    private static HungrySingleton singleton = new HungrySingleton();
    private HungrySingleton(){};

    private static HungrySingleton singleton() {
        return singleton;
    }
}

// 懒汉式 用的时候再创建
class LazySingleton {
    private static LazySingleton singleton;
    private LazySingleton(){};

    private static LazySingleton singleton() {
        if (singleton == null) {
            singleton = new LazySingleton();
        }
        return singleton;
    }
}

// 双重检查锁定
class SafeLazySingleton {
    private static volatile SafeLazySingleton instance;

    private SafeLazySingleton() {}

    public static SafeLazySingleton getInstance() {
        if (instance == null) {
            synchronized (SafeLazySingleton.class) {
                if (instance == null) {
                    instance = new SafeLazySingleton();
                }
            }
        }
        return instance;
    }
```

# MQ消息中间件

![image-20240416044234709](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416044234709.png)

静态方法：

```java
Collections.sort(list);//list集合进行元素的自然顺序排序。
Collections.sort(list,new ComparatorByLen());//按指定的比较器方法排序。
class ComparatorByLen implements Comparator<String>{
    public int compare(String s1,String s2){
        int temp = s1.length()-s2.length();
        return temp==0?s1.compareTo(s2):temp;
    }
}
Collections.max(list);//返回list中字典顺序最大的元素。
int index = Collections.binarySearch(list,"zz");//二分查找，返回角标。
Collections.reverseOrder();//逆向反转排序。
Collections.shuffle(list);//随机对list中的元素进行位置的置换。

//将非同步集合转成同步集合的方法：Collections中的  XXX synchronizedXXX(XXX);  
//原理：定义一个类，将集合所有的方法加同一把锁后返回。
List synchronizedList(list);
Map synchronizedMap(map);
```

# SpringCloud

![](.\img\image-20240416032938611.png)

## SpringCloud有哪些组件


注册nacos
![image-20240416031207214](../../../../Library/Application%20Support/typora-user-images/image-20240416031207214.png)
![image-20240416031326681](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416031326681.png)
![image-20240416032313327](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416032313327.png)
![image-20240416032406682](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416032406682.png)
![image-20240416032454210](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416032454210.png)
![image-20240416032607879](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416032607879.png)
![image-20240416032625087](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416032625087.png)







![image-20240415222814162](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415222814162.png)

## 远程调用

https://vipc9.com/722.html

tyqhua@163.com fujichika4; chenxi

![image-20240403185549958](/Users/chenxi/Library/Application Support/typora-user-images/image-20240403185549958.png)

![image-20240407224408344](/Users/chenxi/Library/Application Support/typora-user-images/image-20240407224408344.png)

![image-20240408000159743](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408000159743.png)

![image-20240408000458468](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408000458468.png)

![image-20240408000700456](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408000700456.png)

![image-20240408000926875](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408000926875.png)

![image-20240408001345630](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408001345630.png)

![image-20240408001420455](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408001420455.png)

![image-20240408001439158](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408001439158.png)

![image-20240408001448764](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408001448764.png)

![image-20240408002427631](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408002427631.png)

![image-20240408002539134](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408002539134.png)

![image-20240408002712136](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408002712136.png)

![image-20240408002845027](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408002845027.png)

QPS每秒请求数3

![image-20240408002944347](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408002944347.png)

## 什么是分布式

分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统。
分布式系统时间里再网络之上的软件系统。
分布式是指将不同的业务分布再不同的地方。
集群是指将几台服务器集中在一起，实现同一业务。
例如：京东是一个分布式系统，众多业务运行再不同的机器，所有业务构成一个大型的业务集群。每一个小的业务，比如用户系统，访问压力大时候一台服务器是不够的。我们就应该将用户系统部署到多个服务器，也就是每一个业务系统也可以做集群化。

分布式中的每一个节点，都可以做集群，而集群并不一定就是分布式的。

节点：集群中的一个服务器。

## 集群

集群是物理形态，分布式是个工作方式。
只要是一堆机器，就可以叫做集群，他们是不是一起写作干活，未知。

## 微服务

微服务架构风格，就像是把一个单独的应用插叙开发炜一套小服务，米格小服务运行再自己的进程中，并使用轻量级机制通信，通常是HTTP  API。这些服务围绕业务能力来构建，并通过完全自动化部署机制来独立部署，这些服务使用不同的编程语言书写，以及不同数据存储技术，并保持最低的集中式管理。
简而言之：拒绝大型单体应用，基于业务边界进行服务微化拆分，各个服务独立部署运行。

## 远程调用

在分布式系统中，各个服务可能处于不同主机，但是服务之间不可避免的需要互相调用，我们称为远程调用。

SpringCloud中使用HTTP+JSON的方式完成远程调用

## 负载均衡

分布式系统中，A服务需要调用B服务，B服务在多台机器中都存在，A调用任意一个服务器均可完成功能。
为了使每一个服务器都不要太忙或者太闲，我们可以负载均衡的调用每一个服务器，提升网站的健壮性。
常见的负载均衡算法：
轮询：为第一个请求选在健康池中的第一个后端服务器，然后按顺序往后依次选择，直到最后一个，然后循环。
最小链接：优先选择链接数最少，也就是压力最小的后端服务器，在会话较长的情况下可以考虑采取这种方式。

## QPS

Queries Per Second，意思是每秒请求数

mysql> show global status where variable_name in('Queries','Uptime');
+---------------+-----------+
| Variable_name | Value     |
+---------------+-----------+
| Queries       | 210350767 |
| Uptime        | 2156149   |
+---------------+-----------+
2 rows in set (0.06 sec)

mysql> show global status where variable_name in('Queries','Uptime');
+---------------+-----------+
| Variable_name | Value     |
+---------------+-----------+
| Queries       | 210356995 |
| Uptime        | 2156150   |
+---------------+-----------+
2 rows in set (0.06 sec)

qps = (210356995 - 210350767 )/(2156150 - 2156149 ) = 6228

## TPS

Transactions Per Second，意思是每秒事务数
事务包含三个过程

==总结==

方式1（推荐）

通过GTID计算事务差异

```sql
SELECT @@global.gtid_executed;
```

方式2

去从库进行统计，计算事务提交和回滚的总和

```sql
show global status where variable_name in('Com_commit','Com_rollback','Uptime');
```

方式3

监听binlog文件的变化进行统计

可以利用MySQL的binlog解析工具，如`mysqlbinlog`或第三方工具（如`Canal`、`Maxwell`等）来解析binlog文件，并提取出其中的事务信息。然后根据事务的提交时间戳和事务包含的操作数量，可以计算出每秒处理的事务量（TPS）

## Zeata

![image-20240415190043337](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415190043337.png)

![image-20240415211044499](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415211044499.png)

全局事务控制

![image-20240415220813760](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415220813760.png)

orderDao更新,然后远程调用reduceStock扣减库存

### XA

Seata对原始的XA模式做了简单的封装和改造，以适应自己的事务模型，基本架构如图：

![img](https://cdn.nlark.com/yuque/0/2023/png/22309163/1697439204802-96936db5-9b2f-4976-ac23-3aab21387d7c.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1135%2Climit_0)

#### RM一阶段的工作：

① 注册分支事务到TC

② 执行分支业务sql但不提交

③ 报告执行状态到TC

#### TC二阶段的工作：

●TC检测各分支事务执行状态a.如果都成功，通知所有RM提交事务b.如果有失败，通知所有RM回滚事务

#### RM二阶段的工作：

●接收TC指令，提交或回滚事务

#### 优缺点 

XA模式的优点是什么？

●事务的强一致性，满足ACID原则。

●常用数据库都支持，实现简单，并且没有代码侵入

XA模式的缺点是什么？

●因为一阶段需要锁定数据库资源，等待二阶段结束才释放，性能较差

●依赖 关系型数据库 实现事务

![image-20240415223738758](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415223738758.png)
![image-20240415223849640](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415223849640.png)
![image-20240415223921186](/Users/chenxi/Library/Application Support/typora-user-images/image-20240415223921186.png)

### AT

AT模式同样是分阶段提交的事务模型，不过缺弥补了XA模型中资源锁定周期过长的缺陷。

Seata的AT模型 

基本流程图：

![img](https://cdn.nlark.com/yuque/0/2023/png/22309163/1697439204776-518ffe46-6400-411f-90da-c21364e38499.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1118%2Climit_0)





#### 阶段一RM的工作：

●注册分支事务

●记录undo-log（数据快照）

●执行业务sql并提交

●报告事务状态

#### 阶段二提交时RM的工作：

●删除undo-log即可

阶段二回滚时RM的工作：

●根据undo-log恢复数据到更新前

#### 流程梳理 

我们用一个真实的业务来梳理下AT模式的原理。

比如，现在又一个数据库表，记录用户余额：

| id   | money |
| ---- | ----- |
| 1    | 100   |

其中一个分支业务要执行的SQL为：

AT模式下，当前分支事务执行流程如下：

#### 一阶段：

1）TM发起并注册全局事务到TC

2）TM调用分支事务

3）分支事务准备执行业务SQL

4）RM拦截业务SQL，根据where条件查询原始数据，形成快照。

5）RM执行业务SQL，提交本地事务，释放数据库锁。此时 money = 90

6）RM报告本地事务状态给TC



#### 二阶段：

1）TM通知TC事务结束

2）TC检查分支事务状态

a）如果都成功，则立即删除快照

b）如果有分支事务失败，需要回滚。读取快照数据（{"id": 1, "money": 100}），将快照恢复到数据库。此时数据库再次恢复为100

#### 脏写问题 

在多线程并发访问AT模式的分布式事务时，有可能出现脏写问题，如图：

![img](https://cdn.nlark.com/yuque/0/2023/png/22309163/1697439204828-f24ec1c3-54b0-44c2-be64-16493e9a7d7c.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1500%2Climit_0)

解决思路就是引入了全局锁的概念。在释放DB锁之前，先拿到全局锁。避免同一时刻有另外一个事务来操作当前数据。

![img](https://cdn.nlark.com/yuque/0/2023/png/22309163/1697439204832-f5c253e1-e466-4786-8dd8-f90e3039e559.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1500%2Climit_0)

\- > 但是也可能在一个极端的情况下造成脏读,比如一个非Seata管理的全局事务，在事务1提交事务释放DB锁之后获取了DB锁，从而造成脏写问题。 > 这时事务1会根据快照数据发现异常，发出警告，进行人工介入。 

#### 优缺点 

AT模式的优点：

●一阶段完成直接提交事务，释放数据库资源，性能比较好

●利用全局锁实现读写隔离

●没有代码侵入，框架自动完成回滚和提交

AT模式的缺点：

●两阶段之间属于软状态，属于最终一致

●框架的快照功能会影响性能，但比XA模式要好很多

## AT与XA的区别?

简述AT模式与XA模式最大的区别是什么？
●XA模式一阶段不提交事务，锁定资源；AT模式一阶段直接提交，不锁定资源。
●XA模式依赖数据库机制实现回滚；AT模式利用数据快照实现数据回滚。
●XA模式强一致；AT模式最终一致

## CAP

![image-20240416074925134](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416074925134.png)

# 并发

## 说下你对volatile的理解

volatile是Java虚拟机提供的轻量级的同步机制，具有以下特点：

### 保证可见性

volatile保证了多个线程对共享变量的操作是可见的。当一个线程修改了共享变量的值，其他线程会立即看到这个改变。

### 禁止指令重排

volatile通过禁止指令重排来保证顺序性。在多线程环境下，为了提高程序执行效率，编译器和处理器可能会对指令进行重新排序。但是，如果一个变量被volatile修饰，就禁止了指令重排，确保每个线程都能看到正确的操作顺序。
总的来说，volatile可以确保多个线程对共享变量的操作一致，避免了数据不一致的问题。但是它不能保证原子性，因此对于需要保证原子性的操作，还需要使用其他同步机制，如synchronized关键字或java.util.concurrent.atomic包中的原子类。

# JVM

![image-20240416044422163](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416044422163.png)

![image-20240416044501867](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416044501867.png)

![image-20240416044730313](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416044730313.png)

![image-20240416045248667](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416045248667.png)

# Nginx

## 整合 Lua 脚本

 Nginx 做反向代理,整合 Lua 脚本用来统计代理支付用户的访问量

Lua模块在NGINX中提供了以下功能：

1 请求处理：你可以使用Lua脚本来修改请求头部、查询参数、请求体等，从而在请求处理过程中进行自定义操作。 
2 响应处理：你可以使用Lua脚本来处理服务器的响应，修改响应内容、响应头部等。 
3 访问控制：通过Lua脚本，你可以实现基于复杂条件的访问控制，例如IP白名单、黑名单，自定义的访问规则等。 
4 缓存控制：使用Lua脚本，你可以实现自定义的缓存控制逻辑，以满足特定的缓存需求。 
5 复杂重定向：Lua模块可以处理更复杂的重定向逻辑，根据请求条件进行不同的重定向操作。 

使用NGINX的Lua模块来定制请求处理流程的步骤如下：

1 安装Lua模块： 首先，确保你已经安装了支持Lua的NGINX模块。你可以通过编译NGINX时启用Lua模块，或者使用预编译的版本，如OpenResty。 

2 在配置中加载Lua模块： 在NGINX配置文件中，使用lua_package_path和lua_package_cpath配置项指定Lua模块的搜索路径。然后使用lua_include指令加载Lua文件。 

```lua
http {
    lua_package_path "/path/to/lua-scripts/?.lua;;";
    lua_package_cpath "/path/to/lua-scripts/?.so;;";

    server {
        location / {
            content_by_lua_file /path/to/lua-script.lua;
        }
    }
}
```

3 编写Lua脚本： 编写Lua脚本文件，其中包含你想要在请求处理过程中执行的自定义逻辑。在Lua脚本中，你可以使用NGINX提供的Lua API来访问请求、响应对象，修改头部信息，执行条件判断等操作。 

4 在配置中调用Lua脚本： 使用content_by_lua_file或access_by_lua_file指令将Lua脚本嵌入到特定的请求处理阶段。这样，在请求到达该阶段时，NGINX会执行Lua脚本中的逻辑。 

# Dubbo

Dubbo是阿里巴巴开源的基于 Java 的高性能RPC（一种远程调用） 分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。

Dubbo是一个服务框架，如果没有分布式的需求，其实是不需要用的，只有在分布式的时候，才有`Dubbo`这样的[分布式](https://so.csdn.net/so/search?q=分布式&spm=1001.2101.3001.7020)服务框架的需求。

并且本质上是个远程服务调用的分布式框架（告别`Web Service`模式中的`WSdl`，以服务者与消费者的方式在`Dubbo`上注册）

## 核心

1、远程通讯：提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求-响应”模式的信息交换方式。
2、集群容错：提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
3、自动发现：基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

## Dubbo能做什么

- 透明化的远程方法调用，就像调用本地方法一样调用远程方法，只需简单配置，没有任何API侵入。
- 软负载均衡及容错机制，可在内网替代F5等硬件负载均衡器，降低成本，减少单点。
- 服务自动注册与发现，不再需要写死服务提供方地址，注册中心基于接口名查询服务提供者的IP地址，并且能够平滑添加或删除服务提供者。

## 调用关系说明

- 容器负责启动，加载，运行服务提供者。
- 服务提供者在启动时，向注册中心注册自己提供的服务。
- 服务消费者在启动时，向注册中心订阅自己所需的服务。
- 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
- 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
- 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

# 面经

## 什么场景,nacos解决什么问题,怎么解决的

是服务注册中心,将接口信息服务注册到nacos,用户端请求进来可以进行转发,在集群模式下,可以配合ribbon进行负载均衡

## seata用到的是哪种模式, AT模式是什么, TM,TC,RM

seata用到的是AT模式,AT模式是分布式事务,开启时会对业务立即提交,不会长时间进行锁住,TM是事务管理者,TC是事务协调者,RM是资源管理者

## 分布式共识算法

## zookeeper的选举机制

有三个或以上的集群服务时,每个服务之间会进行投票,他有zxid和myid都是自增的,首先自己会投自己一票,然后先看zxid,谁大投谁,如果一样就看myid,谁大投谁,得票数最多的选举为leader

## 分布式锁如何实现

首先setnx设置一个唯一值,并且同时设置过期时间,用cas算法进行自旋将并发进行互斥,保证一次只能有一个线程进来,然后执行业务操作,在finnally里面判断是否时自己的锁,如果是就用del进行删除,为了保证原子性,配合lua脚本做了一个原子性的操作,判断的同时进行删除

## redisson的看门狗机制

## 锁用在了事务里面,是如何释放的

## 锁已经释放了,但是事务还没有提交

## mysql的锁

有行锁,表锁

## mysql的隔离级别

RU:读未提交,有脏读问题

RC:读已提交,有不可重复读问题

RR(默认隔离级别):可重复读,有幻读问题

RS:串行化,上面的问题全部解决

## mvcc

mvcc是多版本并发控制,作用在RC和RR两个隔离级别,表中有三个隐藏字段,trx_id(事务id),roll_id(事务指针),当事务进行时,会生成readview快照,快照当中间有三个值,当前事务id,未提交的事务id,和未开始的事务id,当有并发事务时,当前事务会去和readview进行比对,解决并发问题

## 索引有哪几种

普通索引

唯一索引

主键索引

复合索引

聚簇索引

聚集索引

![image-20240416080402958](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416080402958.png)

![image-20240416080617857](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416080617857.png)

## b+tree的结构, 叶子结点

## 如何判断是否用到了索引

explain查看sql执行计划,如果用到索引,会有对应的索引显示

## 排查慢sql

有慢sql日志

## 最多使用到少索引

## cpu飙升较高

## 服务有没有做限流和熔断

## 把方法区内存撑爆

## 查看gc日志,主要关注哪些内容

## 项目中用到了哪些设计模式,解决什么问题,如何解决的

## 微服务抛到docker

## 死锁问题

## Sql优化

## Sql的隔离级别

## 分布式锁 死锁 过期时间

## rocketmq

## redis缓存

## 高并发是如何解决的

## 各种锁

## spring事务失效的各种场景

## 大查询慢是如何解决的

## HashMap，HashTable，ConcurrentHashMap的共同点和区别？

## Spring的 init-method，destroy-method的实现方式？

## 说几种实现幂等的方式？

## JVM内存模型

## MySQL事务隔离的底层实现？

## 手写一个单例

## JVM内存模型，JAVA 栈能分配对象吗？

## 说一下类加载过程，双亲委派模型源码看过吗？介绍一下

## 什么情况下栈会溢出？

## 栈帧介绍一下？

## 垃圾回收了解吗？介绍一下？

## 讲一下AOP的原理？Cglib能代理final方法吗?

## RPC和HTTP协议有什么区别？

## MQ 消息丢了怎么办？发消息是原子操作吗？

## 锁介绍一下？有哪些？

## 线程阻塞从操作系统的角度介绍一下？

## 你的项目如何拆分的？为什么这么拆？

## MySQL挂了怎么办？

## SpringBoot了解吗？和Spring的区别？

## 事务的传播机制？

## 可重入锁的原理？（state ，AQS）

## 如何设计限流？

## 库存怎么扣减？ decrby 可以吗？

## 库存扣减失败怎么办？

## a,b,c 联合索引， a=1,b=1,c>1 能命中索引吗？ a=1,b>1,c=1能命中索引吗？

## JVM 堆说一下？触发Full GC 的场景有哪些？

## 说说G1垃圾回收器？老年代，年轻代如何分配？

## Redis 有哪些持久化方式？

## Redis 数据会存放到磁盘吗？

## Redis为什么这么快？

## Linux 统计top10 IP访问日志，用到哪些命令？（不会）

## Linux 自己用过哪些命令呢？

## 如何查看Dump日志？怎么产生的？命令有哪些？

## 考察一下基础知识吧，String new String 的区别？ （== equals ）

## 项目你认为有哪些难点？（活动报名超员，联想到秒杀）

## 防止超卖还不行，未支付的订单如何处理？

## 用户名密码如何防止被盗？（js加密，不行，还是能破解，哦，https。。。。）

## 说说https的流程？

## 有了解过哪些非对称加密算法？对称加密算法呢？

## 某个业务打爆数据库了怎么办？（分库，拆分服务，单独部署，还有呢？MQ）

## 了解限流吗？降级？算了，你也没接触过，不问了。（ORZ）

## 说一说你对HashMap的结构理解，如果Key相同怎么办，链表是前插还是后插？红黑树呢？

## 1 2 2 3 3 4 4 5，如何确定3的索引位置？ （计数统计，O(N) O(N),还有更好的方法吗？循环吧。（正确答案二分法，我提到了，但是思路却错了，被批了一顿）

## 设计一下Dubbo的线程池？ 每次请求50ms 200 QPS，客户端500ms超时，如何设计？ 直接打入200个请求呢？队列多大？线程池多大？（回答的不好)

## hashmap的底层?

## hashmap的存值与取值?

## mysql的左连接与右连接?

## 一、JVM与性能优化

1. 描述一下 JVM 加载 Class 文件的原理机制?
2. 什么是类加载器？
3. 类加载器有哪些？
4. 什么是tomcat类加载机制？
5. 类加载器双亲委派模型机制？
6. Java 内存分配？
7. Java 堆的结构是什么样子的？
8. 简述各个版本内存区域的变化？
9. 说说各个区域的作用？
10. Java 中会存在内存泄漏吗，简述一下？
11. Java 类加载过程？
12. 什么是GC? 为什么要有 GC？
13. 简述一下Java 垃圾回收机制？
14. 如何判断一个对象是否存活？
15. 垃圾回收的优点和原理，并考虑 2 种回收机制？基本原理是什么？
16. 深拷贝和浅拷贝？
17. 什么是分布式垃圾回收（DGC）？它是如何工作的？
18. 在 Java 中，对象什么时候可以被垃圾回收？
19. 简述Minor GC 和 Major GC？
20. Java 中垃圾收集的方法有哪些？
21. 讲讲你理解的性能评价及测试指标？
22. 常用的性能优化方式有哪些？
23. 说说分布式缓存和一致性哈希？
24. 什么是GC调优？

## 二、Redis

1. redis数据结构有哪些？
2. Redis缓存穿透，缓存雪崩？
3. 如何使用Redis来实现分布式锁？
4. Redis的并发竞争问题如何解决？
5. Redis持久化的几种方式，优缺点是什么，怎么实现的？
6. Redis的缓存失效策略？
7. Redis集群，高可用，原理？
8. Redis缓存分片？
9. Redis的数据淘汰策略？
10. redis队列应用场景？
11. 分布式使用场景（储存session）？

## 三、网络编程

1. TCP建立连接和断开连接的过程？
2. HTTP协议的交互流程• HTTP和HTTPS的差异，SSL的交互流程？
3. TCP的滑动窗口协议有什么用？
4. HTTP协议都有哪些方法？
5. Socket交互的基本流程？
6. 讲讲tcp协议（建连过程，慢启动，滑动窗口，七层模型）？
7. webservice协议（wsdl/soap格式，与restt办议的区别）？
8. 说说Netty线程模型，什么是零拷贝？
9. TCP三次握手、四次挥手？
10. DNS解析过程？
11. TCP如何保证数据的可靠传输的？

## 四、设计模式与重构

1. 说说几个常见的设计模式（23种设计模式）？
2. 设计一个工厂的包的时候会遵循哪些原则?
3. 列举一个使用了 Visitor/ Decorator模式的开源项目/库?
4. 如何实现一个单例?
5. 代理模式(动态代理)？
6. 单例模式(懒汉模式,恶汉模式,并发初始化如何解决, volatile与lock的使用)？
7. JDK源码里面都有些什么让你印象深刻的设计模式使用,举例看看?

## 五、分布式

1. 什么是CAP定理？
2. 说说CAP理论和BASE理论？
3. 什么是最终一致性？最终一致性实现方式？
4. 什么是一致性Hash？
5. 讲讲分布式事务？
6. 如何实现分布式锁？
7. 如何实现分布式 Session?
8. 如何保证消息的一致性?
9. 负载均衡的理解？
10. 正向代理和反向代理？
11. CDN实现原理？
12. 怎么提升系统的QPS和吞吐？
13. Dubbo的底层实现原理和机制？
14. 描述一个服务从发布到被消费的详细过程？
15. 分布式系统怎么做服务治理？
16. 消息中间件如何解决消息丢失问题？
17. Dubbo的服务请求失败怎么处理？
18. 对分布式事务的理解？
19. 如何实现负载均衡,有哪些算法可以实现?
20. Zookeeper的用途,选举的原理是什么?
21. 讲讲数据的垂直拆分水平拆分？
22. zookeeper原理和适用场景？
23. zookeeper watch机制？
24. redis/zk节点宕机如何处理？
25. 分布式集群下如何做到唯一序列号？
26. 用过哪些MQ,怎么用的,和其他mq比较有什么优缺点,MQ的连接是线程安全的吗？
27. MQ系统的数据如何保证不丢失？
28. 列举出能想到的数据库分库分表策略？

![img](https://git.acwing.com/Hasity/interview_hunter/-/raw/master/2024/image/1question.jpg)

![img](https://git.acwing.com/Hasity/interview_hunter/-/raw/master/2024/image/1answer.jpg)

## 一

- 消息队列如何保证可靠性
- 消息队列如何保证消息幂等性
- 消息队列的优缺点
- 为什么用b+树
- 聚集索引和主键区别，其他引擎怎么做的
- 平时数据库编码
- explain参数
- http报文参数有哪些吗？
- 做题，链表奇偶有序输出

## 二

1. 自我介绍
2. 有哪些排序算法？
3. 介绍下快排/堆排/归并排序。
4. 数据库中的索引应该如何设计？
5. 有哪些索引失效的情况？
6. 你们用到的HTTP接口用到了什么提交方式？
7. GET/POST的区别？
8. 除了GET/POST还有哪些？
9. 面向对象的基本原则？再详细说下依赖倒转。
10. 介绍下策略模式和观察者模式？
11. 如何保证用户请求的等幂性？等幂性指的是用户可能连点提交三次支付请求，返回同样的结果（支付成功），但实际后台只执行一次，保持一致性。
12. 介绍下TCP四次挥手？
13. 第四次挥手后客户端是立刻就关闭了吗？是什么状态？
14. 两个大文件，分别每行都存一个url，查找两个文件中重复的url。
15. 一个大文件中，每一行有一个整数，怎么找第100大的数？
16. 一个大文件中，每一行有一个整数，怎么找中位数？
17. redis的基本数据结构？
18. zset是怎么实现的？有哪些命令？
19. 算法题 力扣221. 最大正方形

## 三

- 项目相关（模块划分，项目需求，技术方案，数据库设计，表的结构及关系，担任角色）
- http协议的关键字段，比如request和response头部信息有哪些关键字段，有什么含义
- http状态码：100,200,502,504
- http和https的区别，https是为了解决什么问题
- 三次握手、四次挥手（详细过程+状态变化）
- 出现大量的close_wait可能是什么原因，解决方案，通过什么工具看出来网络有问题等等
- Java中常见的集合有哪些，List、Set、Map初始容量加载因子了解吗
- Java中线程通信的方式有哪些，大概的原理
- MySQL如果遇到性能不好的问题，比如说慢查询，怎么做
- 数据库优化方案（索引 | 分库分表）
- 有哪些索引，数据结构，建立索引的原则
- 分库分表的原则，说说场景（水平 | 垂直、热数据 | 冷数据 blabla）
- 算法题：两数之和

## 四

- 自我介绍、项目介绍，问了数据量
- 了解微服务吗？（有没有自己在做项目时进行调研，了解企业目前常用的工具、方法）
- 了解springcloud吗？
- 一台机器无法满足运载需求，怎么办呢？答：多搞几台机器，问：多台机器如何协同工作？
- 解释一下mapreduce
- 如果有一个很大的文件，TB级别，文件里是乱序的数字，如何排序？mapreduce如何实现？
- 排序过程中的归并排序，请描述一下其过程？时间复杂度
- 进程、线程区别，问使用Java时，里面多线程的概念和os里的线程进程的区别是什么？真正使用时，Java里的线程和进程是如何调度？
- 多线程的同步互斥的方法？答了信号量，问具体怎么实现，答pv操作，给了具体的场景，问变量如何初始化（等同于口述代码）
- b树、b+树是什么样的树结构，查询复杂度？是平衡二叉树吗？
- 使用过redis吗？具体做什么？
- 手撕代码：LRU算法；正反序层序遍历二叉树

# 服务器

120.26.6.24
root
fujichika4;

 外网面板地址: https://120.26.6.24:30562/8b9fffa9
 内网面板地址: https://172.26.8.79:30562/8b9fffa9
 username: w8hvcum5
 password: 25fc6604

==打开面板前请看==

 【云服务器】请在安全组放行 30562 端口
 因默认启用自签证书https加密访问，浏览器将提示不安全
 点击【高级】-【继续访问】或【接受风险并继续】访问
 教程：https://www.bt.cn/bbs/thread-117246-1-1.html

![截屏2024-01-20 15.47.13](/Users/chenxi/Desktop/截屏2024-01-20 15.47.13.png)

已成功部署【WordPress】

数据库账号资料

数据库名：**plwarrior_top**

用户：**plwarrior_top**

密码：**jxfXKS53rz**

访问站点：http://plwarrior.top/index.php

## 1.启动[sshd](https://so.csdn.net/so/search?q=sshd&spm=1001.2101.3001.7020)服务：

sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist

sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist

sudo launchctl list | grep ssh

下面的输出表示成功启动了：

\-   0   com.openssh.sshd

## 4.登录后[中文乱码](https://so.csdn.net/so/search?q=中文乱码&spm=1001.2101.3001.7020)的解决方法：

（4.1）secureCRT连接设置编码为UTF-8编码
（4.2）在/etc/profile最后加入 export LANG=zh_CN.UTF-8
（4.3）修改/etc/profile 提示文件只读。 sudo chmod +w /etc/profile
（4.4）执行 source /etc/profile

## 宝塔面板

外网面板地址:  https://120.26.6.24:35205/7f7083c5
内网面板地址:  https://172.26.8.79:35205/7f7083c5
username: lhe7cjib
password: ********
If you cannot access the panel,
release the following panel port [35205] in the security group
若无法访问面板，请检查防火墙/安全组是否有放行面板[35205]端口

注意：初始密码仅在首次登录面板前能正确获取，其它时间请通过 bt 5 命令修改密码

[root@iZbp19vt872t0iuytf72ovZ ~]# bt 5

正在执行(5)...

请输入新的面板密码：fujichika4;
|-用户名: lhe7cjib
|-新密码: fujichika4;



21)o&TryqHEpoUL$yW

![image-20231016003730486](/Users/chenxi/Library/Application Support/typora-user-images/image-20231016003730486.png)

## mac切换jdk

```
JAVA_HOME_8=/Library/Java/JavaVirtualMachines/jdk1.8.0_361.jdk/Contents/Home
JAVA_HOME_11=/Library/Java/JavaVirtualMachines/jdk-11.jdk/Contents/Home
JAVA_HOME_17=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
JAVA_HOME_20=/Library/Java/JavaVirtualMachines/jdk-20.jdk/Contents/Home

JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME=$JAVA_HOME_8
export JRE_HOME
export PATH
export CLASSPATH

alias jdk8="export JAVA_HOME=$JAVA_HOME_8"
alias jdk11="export JAVA_HOME=$JAVA_HOME_11"
alias jdk17="export JAVA_HOME=$JAVA_HOME_17"
alias jdk20="export JAVA_HOME=$JAVA_HOME_20"
. "$HOME/.cargo/env"
```

## 黑马头条

![image-20240120154905984](/Users/chenxi/Library/Application Support/typora-user-images/image-20240120154905984.png)

![image-20240120155506207](/Users/chenxi/Library/Application Support/typora-user-images/image-20240120155506207.png)

![image-20240120155924429](/Users/chenxi/Library/Application Support/typora-user-images/image-20240120155924429.png)

![image-20240120160027371](/Users/chenxi/Library/Application Support/typora-user-images/image-20240120160027371.png)

```
责订单模块的开发，通过SpringBoot+策略模式，灵活实现不同类型用户，不同场景下订单的业务场景处理; 通过Redis实现锁，防止恶意用户频繁下单。负责与第三方物流功能，支付/代扣功能，短信功能的功能对接，通过 WebSocket功能实现物流路由信息的主动推送; 使用RabbitMQ确保高并发下用户短信的正常收发;。 负责会员业务中会员卡的激活，核销，会员优惠券的使用流程的功能实现，使用规则引擎Drools实现各种会员权益 的决策，简化原代码中大量的if-else判断流程。
```

![image-20240323215655438](/Users/chenxi/Library/Application Support/typora-user-images/image-20240323215655438.png)

## cpu标高

top 
top -H -p  进程ID
转换16进制线程pid：printf '0x%x\n' 进程ID
jstack 进程ID ｜grep 16进制线程pid -A20

## linux服务器



![image-20240328163309124](/Users/chenxi/Library/Application Support/typora-user-images/image-20240328163309124.png)

root

![image-20240328163425696](/Users/chenxi/Library/Application Support/typora-user-images/image-20240328163425696.png)

## 定位线上OOM问题

![image-20240416080800769](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416080800769.png)
