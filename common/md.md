# Typora快捷键

![image-20220709213532700](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220709213532700.png)![image-20220709213549038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220709213549038.png)![image-20220709213613213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220709213613213.png)

```xml
切换源码模式
Ctrl+/ 

代码块
Ctrl+Shift+k

1-6标题
Ctrl+123456

引用
Ctrl+Shift+Q

插入图片
ctrl+shift+i

超链接
ctrl+k
```

## ==换行==

```xml
shift+enter or <br/> 换行
```

## ==高亮==

```undefined
==高亮显示== 
```

Emoji

```ruby
:smile: 表情
```

选中当前行

```undefined
ctrl+L
```

关闭窗口

```undefined
ctrl+w
```



# markdown语法

```java
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
## java代码
​```java
    @Override
    public String toString() {
        return "Student{" +
                "stuId=" + stuId +
                ", name='" + name + '\'' +
                ", score=" + score +
                '}';
    }  
​```
## sql代码
1. 查询每个部门中年龄最小的员工
​```sql
SELECT
 MAX(worker_date) 最小年龄,
 worker_name,
 worker_department
FROM
 worker
GROUP BY
 worker_department
​```
## 字体
### 没加粗
James Gosling
### 加粗
**James Gosling**

### 代码非高亮显示
James Gosling
### 代码高亮显示
==代码高亮显示==
### 删除线 
~~被删除的文字~~
### 斜体
*斜体的文字*

## 引用
>作者；陈玺  
### 分割线
分割线一：
---
分割线二：
***

### 超链接  
[我的GitHub](https://github.com/iwfangames/iw-fangames/releas%E6%88%91es/)

### 图片
![我的照片](D:\Administrator\Photos\Lu Xun\timg.jpg)

### 列表  
- 目录一
- 目录二
- 目录三
1. 目录四
2. 目录五
3. 目录六

### 表格
课程|Java|mysql|vue
```



# 标题

```javascript
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

# 一级标题

## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
# java代码

```javascript
```java
    @Override
    public String toString() {
        return "Student{" +
                "stuId=" + stuId +
                ", name='" + name + '\'' +
                ", score=" + score +
                '}';
    }  
​```
```

```java
    @Override
    public String toString() {
        return "Student{" +
                "stuId=" + stuId +
                ", name='" + name + '\'' +
                ", score=" + score +
                '}';
    }  
```
# 加粗

```java
**James Gosling**
```

**James Gosling**

# 引用

>作者:陈玺  

# 代码高亮显示

```javascript
==代码高亮显示==
```

==代码高亮显示==

# 删除线 

```javascript
~~被删除的文字~~
```

~~被删除的文字~~

# 斜体

```javascript
*斜体的文字*
```

*斜体的文字*

# 分割线一

```javascript
分割线一：
---
```

***

# 分割线二

```javascript
分割线二：
***
```

# 列表一

```javascript
- 目录一
- 目录二
- 目录三
```

- 目录一
- 目录二
- 目录三

# 列表二

```javascript
1. 目录四
2. 目录五
3. 目录六
```

1. 目录四
2. 目录五
3. 目录六
