# 用户与权限系统设计
## 权限系统概要
在用户系统中，包括组织(organization)与用户(user)两个维度，与组织权限(org_authorities)与用户权限(user_authorities)两个维度。其中，权限在数据库中存储为字符串，在签发access_token或在服务层鉴权时，相同范围下(如所有的组织权限，同一个组织下的所有用户权限等)的权限使用uint64进行二进制编码表示。假设二进制位数为8，权限1的二进制码为0x1(00000001)，权限2的二进制码为0x2(00000010)，以此类推。

存储在access_token中的authorities结构为 map[uint64]uint64，即 map[组织权限]用户权限。
当组织权限被吊销时，用户权限将不被签发在token中。

为方便管理，另有组织角色(org_role)与用户角色（user_role）对authorities进行管理，用户角色从属于某个组织。每个组织、用户仅能够被赋予唯一的角色。
## 数据库设计
### 组织
字段 | 类型 | 描述
-----|-----|----
id| int64| id
name| varchar(60)|组织名称
### 组织权限
字段 | 类型 | 描述
-----|-----|----
id| int64| 组织id
key| uint64| 组织对应的二进制key
value|varchar(60)|值
name| varchar(60)|名称
### 组织角色
字段 | 类型 | 描述
-----|-----|----
id | int64| 组织角色id
name|varchar(50)|角色名称
authority_id|int64|权限id
### 用户(user)

字段 | 类型 | 描述
-----|-----|----
id | int64| 用户id,使用分布式id生成算法
org_id|int64| 组织 id
login| varchar(50)| 登录名
email| varchar(254)| 邮箱
phone| varchar(50)| 手机号
password_hash|varchar(60)| 密码
activated| bool | 激活
lang_key | varchar(6)| 语言
avatar_url|varchar(256)| 头像
activation_key|varchar(20) | 激活key
reset_key|varchar(20) | 充值key
reset_date|time | resetDate

### 用户权限(user_authorities)
字段 | 类型 | 描述
-----|-----|----
id | int64| 权限id
key | uint64| 权限对应的二进制key
org\_auth\_id|int64|组织权限id
value|varchar(60)|权限值
name|varchar(60)|权限名称
### 用户角色
字段 | 类型 | 描述
-----|-----|----
id | int64| 角色id
org_id | int64| 对应的组织id
name|varchar(50)|角色名称
authority_id|int64|权限id
