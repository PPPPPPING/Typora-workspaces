# 呼叫-知识库

## 我的知识库-创建知识库

### 调用dubbo接口查询 部门表(部门接口)

| 出参 |          |
| ---- | -------- |
|      | 部门id   |
|      | 部门名称 |

### 新增知识库接口

kd_knowledge_base + kd_visible_scope

| 入参     |                                                              |
| -------- | ------------------------------------------------------------ |
| kd_name  | 知识库名称                                                   |
| kd_intro | 知识库简介                                                   |
| kd_type  | '知识库类型 00-普通知识库 01-应答知识库'                     |
|          |                                                              |
| dept_id  | 部门ID(调用部门接口)(新增kd_visible_scope : kd_id ,  dept_id) |

| 出参         |              |
| ------------ | ------------ |
| create_by    | 创建人       |
| create_by_id | 创建人用户id |
| create_at    | 创建时间     |
| update_at    | 更新时间     |
| kd_logo      | 知识库logo   |

### 更新权限接口(编辑知识库权限结接口)

kd_member_info

| 入参    |        |
| ------- | ------ |
| user_id | 成员ID |

| 出参           |                         |
| -------------- | ----------------------- |
| authority_code | 权限编码 对应数据字典   |
| kd_id          | 知识库ID                |
| create_at      | 创建时间                |
| update_at      | 更新时间                |
| is_creator     | 是否是创建人 0-不是1-是 |



## 查询我的知识库

### 调用登录接口

| 出参    |        |
| ------- | ------ |
| user_id |        |
| dept_id | 部门id |

### 查询我的知识库接口

kd_knowledge_base + kd_visible_scope

| 入参    |        |
| ------- | ------ |
| dept_id | 部门id |

| 出参         |            |
| ------------ | ---------- |
| kd_name      | 知识库名称 |
| kd_intro     | 知识库简介 |
| kd_logo      | 知识库logo |
| create_by    |            |
| create_by_id |            |
| create_at    |            |
| update_at    |            |

## 我的知识库-设置我的知识库

### 上传图片的接口

### 查询是否有可编辑知识库的权限接口(查询知识库权限接口)

| 入参                  |            |
| --------------------- | ---------- |
| user_id               | 成员用户id |
| kd_knowledge_base(id) | 知识库id   |

| 出参           |                         |
| -------------- | ----------------------- |
| authority_code | 权限编码 对应数据字典   |
| kd_id          | 知识库ID                |
| create_at      | 创建时间                |
| update_at      | 更新时间                |
| is_creator     | 是否是创建人 0-不是1-是 |

### 根	据知识库id查询知识库信息接口 

kd_knowledge_base	

| 入参 |          |
| ---- | -------- |
| id   | 知识库id |

| 出参         |                                        |
| ------------ | -------------------------------------- |
| kd_name      | 知识库名称                             |
| kd_intro     | 知识库简介                             |
| kd_type      | 知识库类型 00-普通知识库 01-应答知识库 |
| create_by    | 创建人                                 |
| create_at    | 创建时间                               |
| update_at    | 更新时间                               |
| create_by_id | 创建人用户id                           |
| t_id         | 知识库归属的租户                       |
| kd_logo      | 知识库logo                             |
| remark       | 备注                                   |

### (部门接口)

### 编辑知识库设置接口

kd_knowledge_base + kd_visible_scope

| 入参     |            |
| -------- | ---------- |
| kd_logo  | 知识库logo |
| kd_name  | 知识库标题 |
| kd_intro | 知识库简介 |
|          |            |
| dept_id  | 部门ID     |

### (编辑知识库权限结接口)

### 成员管理

#### 添加成员

##### (部门接口)

##### 调用dubbo查询成员接口

| 入参 |      |
| ---- | ---- |
| 姓名 |      |
| 部门 |      |

| 出参 |          |
| ---- | -------- |
|      | 姓名     |
|      | 工号     |
|      | 头像     |
|      | 所属部门 |

##### 立即添加新增接口==(编辑知识库权限结接口)

kd_member_info

| 入参           |                                 |
| -------------- | ------------------------------- |
| user_id        | 成员用户ID                      |
| authority_code | 默认给可编辑权限                |
| is_creator     | 是否是创建人 0-不是1-是 默认为0 |

#### 成员权限修改接口	==(	编			辑知识		库								权限结接口)

kd_member_info

| 入参           |                       |
| -------------- | --------------------- |
| authority_code | 权限编码 对应数据字典 |

#### 查询成员接口

kd_member_info

| 入参                         |          |
| ---------------------------- | -------- |
| kd_knowledge_base(id)>>kd_id | 知识库id |

#### (查询知识库权限接口)

| 出参                    |      |
| ----------------------- | ---- |
| kd_member_info(user_id) |      |
| authority_code          |      |
| is_creator              |      |

#### 删除接口

kd_member_info

| 入参    |        |
| ------- | ------ |
| user_id | 成员id |

## 我的知识库-普通库-文档

### 知识库标题查询搜索(如果是知识库成员就是查kd_doc , 如果是不是成员就查kd_doc_publish)

#### 判断是否是知识库成员接口(查询知识库权限接口)

(查询知识库权限接口)>>传入user_id , 查询kd_member_info中的kd_id , 判断kd_doc里面的kd_id有没有kd_member_info中的kd_id

### 知识库成员

#### (查询知识库权限接口)

#### (查询文档信息接口)

kd_doc

| 入参    |                  |
| ------- | ---------------- |
| kd_id   | 知识库ID         |
| p_dc_id | 父文档id(非必填) |

| 出参            |                                              |
| --------------- | -------------------------------------------- |
| title           | 文档标题                                     |
| content         | 文档内容                                     |
| cstate          | 状态 00-已发布 01-编辑中 02-待发布 03-已删除 |
| create_by       | 创建人                                       |
| create_by_id    | 创建人ID                                     |
| look_count      | 浏览量                                       |
| p_dc_id         | 父文档ID(list)                               |
| last_editor     | 最后编辑人                                   |
| last_editor_id  | 最后编辑人ID                                 |
| last_opt_status | 最后一次的操作状态                           |

#### 知识库成员-知识库标题查询搜索

kd_doc

| 入参  |          |
| ----- | -------- |
| kd_id | 知识库id |
| title | 标题     |

| 出参            |          |
| --------------- | -------- |
| id              |          |
| title           | 文档标题 |
| content         | 文档内容 |
| cstate          |          |
| create_by       |          |
| create_by_id    |          |
| look_count      |          |
| p_dc_id         |          |
| last_editor     |          |
| last_editor_id  |          |
| last_opt_status |          |

####  新增文档接口

kd_doc

| 入参            |      |
| --------------- | ---- |
| kd_id           |      |
| title           |      |
| content         |      |
| t_id            |      |
| ownership       |      |
| create_by       |      |
| create_by_id    |      |
| p_dc_id         |      |
| last_editor     |      |
| last_edit_time  |      |
| last_editor_id  |      |
| last_opt_status |      |

#### (文档修改接口)

#### 发布文档接口

新增kd_doc_publish  /   修改 (是否传id)

| 入参              |        |
| ----------------- | ------ |
| kd_doc(id)>>kd_id | 文档id |
| publisher_id      |        |
| publisher         |        |
| create_at         |        |
| content           |        |
| title             |        |
| update_at         |        |
| cversion          |        |

### 删除接口

kd_doc删除

| 入参   |                                              |
| ------ | -------------------------------------------- |
| id     |                                              |
| cstate | 状态 00-已发布 01-编辑中 02-待发布 03-已删除 |

kd_doc_publish删除

kd_doc_publish  +  kd_doc  where  cstate = 已发布

### 非知识库成员

#### (非知识库成员查询文档接口)

kd_doc_publish + kd_doc

| 入参               |        |
| ------------------ | ------ |
| kd_doc(id)>>doc_id | 文档ID |

| 出参            |                    |
| --------------- | ------------------ |
| publisher_id    | 发布人ID           |
| publisher       | 发布人             |
| create_at       |                    |
| content         |                    |
| title           |                    |
| update_at       |                    |
| cversion        |                    |
|                 |                    |
| look_count      | 浏览量             |
| p_dc_id         | 父文档ID           |
| last_editor     | 最后编辑人         |
| last_edit_time  | 最后编辑时间       |
| last_editor_id  | 最后编辑人ID       |
| last_opt_status | 最后一次的操作状态 |

#### 非知识库成员-知识库标题查询搜索接口(非知识库成员查询文档接口)

### 修改记录接口(预览,恢复)

每改变一次状态就新增一条记录

kd_doc_opt_log

| 入参      |                                                        |
| --------- | ------------------------------------------------------ |
| doc_id    | 文档id                                                 |
| opt_type  | 操作类型 0-新增 1-编辑 2-申请发布 3-发布 4-删除 5-恢复 |
| opt_by    | 操作员                                                 |
| opt_by_id | 操作员ID                                               |
| content   | 文档内容                                               |
| title     | 文档标题                                               |

## 我的知识库-回收站

### (查询知识库权限结接口)				

### 回收站文章记录接口(查询文档信息接口)

### 回收站文章预览接口(查询文档信息接口)

### 恢复(修改)文档接口(文档修改接口)

## 我的知识库-应答库-文档

### (查询知识库权限结接口)

### (查询文档信息接口)

kd_doc

| 入参    |          |
| ------- | -------- |
| kd_id   | 知识库ID |
| p_dc_id | 1?>      |

| 出参            |                                              |
| --------------- | -------------------------------------------- |
| title           | 文档标题                                     |
| content         | 文档内容                                     |
| cstate          | 状态 00-已发布 01-编辑中 02-待发布 03-已删除 |
| create_by       | 创建人                                       |
| create_by_id    | 创建人ID                                     |
| look_count      | 浏览量                                       |
| p_dc_id         | 父文档ID(list)                               |
| last_editor     | 最后编辑人                                   |
| last_editor_id  | 最后编辑人ID                                 |
| last_opt_status | 最后一次的操作状态                           |
|                 |                                              |
| ckeyword        | 关键字                                       |
| create_at       | 创建时间                                     |
| update_at       | 更新时间                                     |
| del_flag        | 删除标识 0-启用 1-删除                       |

### (文档修改接口)

## 知识库搜索

### (部门接口)

### 知识库标题搜索接口-知识库标题查询搜索(如果是知识库成员就是查kd_doc , 如果是不是成员就查kd_doc_publish)

(查询知识库权限接口)>>传入user_id , 查询kd_mem	ber_info中的kd_id , 判断kd	_doc里面的kd_id有没有kd_member_info中的kd	_id

kd_doc

| 入参    |          |
| ------- | -------- |
| title   | 文档标题 |
|         |          |
| dept_id | 部门id   |

| 出参    |          |
| ------- | -------- |
| id      |          |
| kd_id   | 知识库id |
| title   | 文章标题 |
| content | 文章内容 |
