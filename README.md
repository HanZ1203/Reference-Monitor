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
- 请求路径：非资源端点的路径，如`/api`。
- 资源路径：访问的资源的路径（仅限资源请求）。
### DSL
用于描述Capability配置。

属性定义：
- 版本控制属性：Version，字符串。
- 具有映射关系的属性：spec
  - 主体匹配属性：UID，字符串。
  - 资源匹配属性：resourceAttributes
    - 资源路径：resourcePath，字符串。
    - readonly：键入布尔值，如果为 true，则表示该策略仅可读。
  - 非资源匹配属性：nonResourceAttributes
    - 请求路径：nonResourcePath，字符串。
    - 请求动词：verb，字符串。

策略定义：

a123账户可以读取任何资源
```
{"Version": "abac.v1", "spec": {"uid": "a123", "resourcePath": "*", "readonly": true}}
```

请求示例：
```
{
  "apiVersion": "abac.v1",
  "spec": {
  "token": "a123token",
    "resourceAttributes": {
      "verb": "get",
      "resourcePath": "/storage"
    }
  }
}
```
### 策略配置规则
策略获取
- offered by parents：继承而来的权限配置，与父辈应用权限配置内容一样。
- offered by default：属于自己量身定做的权限配置。

策略发放
- exposed by app：由应用发放，一个应用只能发放自己拥有的权限。
- exposed by default：由用户定义或运营商定义。
## Reference Monitor
用于保证访问控制策略的顺利实施，即所有请求必须通过上述访问控制策略。
### 定义
- Non-bypassable：不可以被攻击者避开。
- Evaluable：能够被管理者管理、测试和控制。
- Always invoked：时刻被调用。
- Tamper-proof：不易受到攻击。
### 实现
在连接建立阶段完成认证，即先认证后连接，避免来自非法身份的连接。

在运行时完成授权，检查请求中的属性元组，与每一条策略进行匹配，若匹配成功则授权。
