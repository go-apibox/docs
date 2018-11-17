API CSRF防护
============

### 实现原理

当 API 调用仅使用 `apibox/session` 中间件进行 Session 验证时，攻击者很容易利用 CSRF 方式，在站外对 API 进行跨站访问攻击。

因此，APIBox 提供了 CSRF 防护中间件，对此类攻击进行拦截。

在实现上，每个 CSRF TOKEN 都是唯一的，并且绑定特定的 Session。

CSRF 防护功能的实施，需要前后端的一起配合才能实现，主要实现以下两个流程：

**CSRF TOKEN 的生成：**

前端通过 API 请求调用后端 API 生成 CSRF TOKEN，并将请求结果中的 TOKENID 保存在 JS COOKIE 中。
一般与 apisession 中间件共同使用，在用户登录成功后返回 CSRF TOKEN。

**CSRF TOKEN 的验证：**

使用此中间件后，API 系统会对 CSRF TOKEN 进行检测。因此，前端在 API 调用时，需要把 CSRF TOKEN 附在 HTTP HEADER 中，传递给后端用于检测。


### 相关配置

- apicsrf：
   API CSRF 防护中间件配置组

  - disabled:
    是否禁用该中间件，不配置则默认为：`false`。

  - http_header：
     HTTP HEADER 中 CSRF TOKEN 的名称，不配置则默认为：`X-CSRF-TOKEN`。
	
  -  session_store_key：
     指定服务端 CSRF TOKEN ID 要保存在哪个 Session 值中，格式为：`session名称.session键名`，不配置则默认为：`default.csrf_token`。

  - actions：
      接口匹配规则。

     - whitelist：
        接口白名单：要进行 CSRF 检测的接口名称列表，默认会对所有接口进行 CSRF 检测。
        支持通配符 `*`。

     - blacklist：
        接口黑名单：要忽略 CSRF 检测的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行 CSRF 检测。


配置示例：

	apicsrf:
	  http_header: X-CSRF-TOKEN
	  session_store_key: default.csrf_token
	  actions:
	    blacklist:
        - Public.Action

### 相关错误

| 错误码                 | 错误消息                    | 错误描述         |
| --------------------- | -------------------------- | --------------- |
| SessionInitFailed     | Session init failed!       | 会话初始化失败！   |
| SessionGetFailed      | Failed to get session!     | 会话获取失败！    |
| CSRFTokenError        | CSRF token error!          | CSRF验证失败！    |

### 接口参考

- `func (cs *CSRF) Enable()`
  启用该中间件。

- `func (cs *CSRF) Disable()`
  禁用该中间件。

### 使用示例

**应用入口：**

    package main
    
    import (
    	"os"
    
    	"git.quyun.com/apibox/api"
		"git.quyun.com/apibox/apisession"
    	"git.quyun.com/apibox/apicsrf"
    )
    
    func main() {
    	app, err := api.NewApp()
    	if err != nil {
    		println(err.Error())
    		os.Exit(1)
    	}
    
		app.Use(apisession.NewSession(app))
    	app.Use(apicsrf.NewCSRF(app))
    	app.Run(apiRoutes)

**中间件配置：**

	apisession:
	  actions:
	    blacklist:
	    - Login
	  
	apicsrf:
	  actions:
	    blacklist:
	    - Login

**登录与注销：**

	package main
	
	import (
		"git.quyun.com/apibox/api"
		"git.quyun.com/apibox/utils"
	)
	
	func LoginAction(c *api.Context) interface{} {
		session, err := c.Session("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "SessionFailed").SetMessage(err.Error())
		}
	
		// 标识用户已验证
		session.Set("authed", true)
	
		// 生成随机的CSRF Token
		csrfToken := utils.RandString()
		session.Set("csrf_token", csrfToken)
		session.Save()
	
		return utils.Combine("CSRFToken", csrfToken)
	}
	
	func LogoutAction(c *api.Context) interface{} {
		session, err := c.Session("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "SessionFailed").SetMessage(err.Error())
		}
	
		session.Destroy()
	
		return true
	}

