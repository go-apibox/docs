API Session验证
==============

### 实现原理

API Session机制，是验证调用者身份的一种手段，是通过对请求中携带的 Session 数据进行验证实现的。

该中间件使用 apibox/session （使用加密的 Cookie 作为 Session 存储）作为底层 Session 实现，具体可参考文档：

 - [Session类库](../session/session.md)

### 相关配置

- apisession：
    API Session验证中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。
      
    - auth_key：
      用于判断是否有权限使用的 Session 值，格式为：`session名称.session键名` 或 `session键名`。
      如果没有指定 session 名称，则 session 名称默认为 `default`，如果不配置则默认为：`default.authed`。
      当 Session 中该键的值为 true 时表示认证成功，否则表示认证失败。

    - actions：
      接口匹配规则。

      - whitelist：
        接口白名单：要进行 session 验证的接口名称列表，默认会对所有接口进行 session 验证。
        支持通配符 `*`。

      - blacklist：
        接口黑名单：要忽略 session 验证的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行 session 验证。
      
配置示例：

	apisession:
	  auth_key: authed
	  actions:
	    blacklist:
	    - Login
	    - Public.Action

### 相关错误

| 错误码                 | 错误消息                    | 错误描述         |
| --------------------- | -------------------------- | --------------- |
| SessionInitFailed     | Session init failed!       | 会话初始化失败！   |
| SessionGetFailed      | Failed to get session!     | 会话获取失败！    |
| SessionNotAuthed      | Session is not authed!     | 会话未认证！      |

### 接口参考

- `func (s *Session) Enable()`
  启用该中间件。

- `func (s *Session) Disable()`
  禁用该中间件。

###使用示例：

**应用入口：**

	package main
	
	import (
		"os"
	
		"github.com/go-apibox/api"
		"github.com/go-apibox/apisession"
	)
	
	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apisession.NewSession(app))
		app.Run(apiRoutes)
	}

**中间件配置：**

	apisession:
	  ignore_actions:
	  - Login

**登录/注销：**

	package main

	import (
		"github.com/go-apibox/api"
		"github.com/go-apibox/utils"
	)
	
	func LoginAction(c *api.Context) interface{} {
		session, err := c.Session("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "SessionFailed").SetMessage(err.Error())
		}
	
		// 标识用户已验证
		session.Values["authed"] = true
		session.Save()
	
		return true
	}
	
	func LogoutAction(c *api.Context) interface{} {
		session, err := c.Session("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "SessionFailed").SetMessage(err.Error())
		}
	
		session.Destroy()
	
		return true
	}
	
