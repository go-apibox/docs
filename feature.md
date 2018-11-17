功能特色
=========

APIBox 由两个子框架构成：

 - `apibox/api`：用于快速构建 API 系统
	 - 基于请求参数 `api_action` 的路由
	 - 支持请求参数验证，支持多种常见 API 参数类型
		 - 该特性通过 `apibox/filter` 实现
	 - 支持数据库便捷操作，便捷输出 API 响应结果

 - `apibox/web`：用于快速构建基于 API 的 Web 系统
	 - 支持 API 代理请求，将收到的 API 请求代理到后端的 API 系统
	 - 支持 API 请求的 CSRF 防御
	 - 支持 Session 验证，可配置未登录的页面自动跳转
	 - 支持前端协作，将路由控制权交给前端开发人员

APIBox 还实现了丰富的中间件：

 - `apibox/apisign`：API签名验证中间件
 - `apibox/apinonce`：API重攻击防护中间件
 - `apibox/apilog`：API日志记录中间件
 - `apibox/apitoken`：API幂等性中间件
 - `apibox/apiproxy`：API代理调用中间件
 - `apibox/apisession`：API Session验证中间件
 - `apibox/apicsrf`：API CSR 防护中间件

另外实现了工具类库：

- `apibox/filter`：用于快速对请求参数进行验证
	- 支持多种常见类型的验证
	- 支持自定义验证规则

- `apibox/session`：基于加密 Cookie 的 Session 实现
  - 支持定期自动更新加密 Cookie 使用的密钥