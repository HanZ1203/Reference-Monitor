# Reference-Monitor
## 访问控制策略
基于属性的访问控制策略（ABAC），访问控制策略两个步骤的安全措施：认证和授权。

认证解决请求者是谁的问题，授权解决请求者能做什么的问题。通过合理的权限管理来保证系统的安全可靠。
### 认证
规则：当接收到请求时，确定账号，未知账号统一设置为游客账号，由账号进行认证。

账号管理方案：
- 使用包含账号ID和密码列表的文件。
- 使用类似Google Accounts的用户数据库。

字段：账号ID、账号密码、Token。
### 授权
规则：当接收到请求时，确定属性，由属性值进行授权。未知属性设置为其类型的零值（例如：空字符串，0，false），设置为`"*"`的属性将匹配相应属性的任何值。

权限管理方案：使用包含Capability的Policy策略文件进行权限匹配。

字段：
- 用户：身份验证期间提供的账号ID。
- 请求动词：get、update、patch、delete等，区分资源请和非资源请求。
- 请求路径：各种非资源端点的路径，如`/api`。
- 请求资源：正在访问的资源的ID或名称（仅限资源请求）。
### DSL
用于描述Capability配置。

属性定义：
- 版本控制属性：Version，字符串。
- 具有映射关系的属性：spec
  - 主体匹配属性：UID，字符串。
  - 资源匹配属性：
    - 资源ID：resource，字符串。
    - readonly：键入布尔值，如果为 true，则表示该策略仅可读。
  - 非资源匹配属性：
    - 请求路径：nonResourcePath，字符串。
    - 请求动词：verb，字符串。

策略定义：

a123账户可以读取任何资源
```
{"Version": "abac.v1", "spec": {"uid": "a123", "resource": "*", "readonly": true}}
```

请求示例：
```
{
  "apiVersion": "abac.v1",
  "spec": {
  "token": "a123token",
    "resourceAttributes": {
      "verb": "get",
      "resource": "storage1"
    }
  }
}
```
## Reference Monitor
### 定义
### 实现
