[toc]

# 有没有oom，怎么解决的

## 21 先分析还是先扩容，还是立刻重启

## 22 重启了你们的现场还在吗

# jvm的内存主要区域

# JVM内存模型

# jvm分区

# 垃圾回收算法

## jvm垃圾回收器在项目里面用的哪个

# 类的加载过程

触发加载动作：Class.forName(),ClassLoader.loadClass,或者JVM触发，获得类的全限定名，传给类加载器，类加载器通过类名找本地硬盘上的类文件、网络上的类文件、运行时计算生成字节码流，获取到字节码的二进制流，根据字节码的二进制流创建加载对应的class对象

可以通过类名.class.getClassLoader()来获取这个类的加载器：AppClassLoader加载器，负责加载当前java应用classpath中的类，，ExtClassLoader扩展类加载器，扩展目录通常是<JAVA_HOME>lib/ext中得到，启动加载器，Bootstrap ClassLoader，加载核心类库<JAVA_HOME>/jre/lib中的rt.jar

Starter：一站式服务的依赖jar包，包含spring以及相关技术的所有依赖，提供了自动配置的功能，开箱即用，提供了良好的依赖管理，避免包遗漏，版本冲突问题

核心容器创建、配置、管理bean

# 垃圾回收器的特性

# jvm的配置指的是哪些

# 怎么看GC的情况

# 线上jvm内存多大

