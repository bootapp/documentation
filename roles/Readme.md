权限系统设计
在用户系统中，包括组织(organization)与用户(user)两个维度，与组织权限(org_authorities)与用户权限(user_authorities)两个维度。其中，权限在数据库中存储为字符串，在签发access_token或在服务层鉴权时，相同范围下(如所有的组织权限，同一个组织下的所有用户权限等)的权限使用uint64进行二进制编码表示。假设二进制位数为8，权限1的二进制码为0x1(00000001)，权限2的二进制码为0x2(00000010)，以此类推。
存储在access_token中的authorities结构为 map[uint64]uint64，即 map[组织权限]用户权限。
当组织权限被吊销时，用户权限将不被签发在token中。
为方便管理，另有组织角色(org_role)与用户角色（user_role）对authorities进行管理，用户角色从属于某个组织。每个组织、用户仅能够被赋予唯一的角色。
