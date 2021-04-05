# Reference-Monitor
## 访问控制策略
访问控制策略两个步骤的安全措施：认证和授权。
认证解决用户是谁的问题，授权解决用户能做什么的问题。通过合理的权限管理来保证系统的安全可靠。
### 认证
规则：
当接收到请求时，确定账号，未知账号统一设置为游客账号，由用户账号进行认证。
账号管理方案：
-使用包含用户名和密码列表的文件。
-使用类似Google Accounts用户数据库。
字段：账号ID、账号密码、Token。
### 授权
规则：
当接收到请求时，确定属性，未知属性设置为其类型的零值（例如：空字符串，0，false），由属性值进行授权。
权限管理方案：使用包含Capability的Policy策略文件进行权限匹配。
字段：
-用户：身份验证期间提供的账号ID。
-请求动词：get、update、patch、delete等用于资源请求的请求动词；post、put等用于非资源请求的请求动词。
-请求路径：各种非资源端点的路径，如/api。
-请求资源：正在访问的资源的ID或名称（仅限资源请求）。
### DSL
## Reference Monitor
### 定义
### 实现
