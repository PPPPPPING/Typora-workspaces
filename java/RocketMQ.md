## Rocketmq

同步发送:

消费者:发送消息会有一个返回值,判断返回值是否错误,错误的话进行重新发送

![image-20240417173334676](/Users/chenxi/Library/Application Support/typora-user-images/image-20240417173334676.png)

### 如何保证消息不会丢失

![image-20240417180234486](/Users/chenxi/Library/Application Support/typora-user-images/image-20240417180234486.png)

![image-20240416043541742](/Users/chenxi/Library/Application Support/typora-user-images/image-20240416043541742.png)

### 如何保证消息有序性

![image-20240417183304454](/Users/chenxi/Library/Application Support/typora-user-images/image-20240417183304454.png)

## 如何保证线上消息不丢失?

![image-20240408165836826](/Users/chenxi/Library/Application Support/typora-user-images/image-20240408165836826.png)

异步刷盘改成同步刷盘, 进入磁盘再进行发送给生产者, 异步是放在内存的, 效率高

消费者用ACK确认机制, 消息成功处理, 再返回一个确认, 性能下降, 消息重复, rocket mq消息自动重试机制

## 
