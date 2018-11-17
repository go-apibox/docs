API代理请求
============

### 实现原理

如果希望将多个基于 APIBox 开发的 API 系统汇集到同一个 API 系统中，可以使用 API 代理中间件来实现。

apiproxy 通过检测请求参数中的参数值来决定将请求代理到某个后端的 API 系统中，在后端 API 系统返回请求结果后，再将请求结果原封不动地返回给当前系统的调用者。

### 相关配置

- apiproxy：
    API代理请求中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。

    - trusted_rproxy_ips:
      受信任的反向代理IP列表。

    - backends:
      后端列表。

      - {api系统别名}：
	     在此定义一个后端 API 系统的别名。

        - gwurl：
          后端 API 系统的网关地址，不得为空。
          如果为空则该后端 API 系统配置将被忽略。
          如果该值为 `@`，则表示该API系统为本地 API 系统，不需要代理到后端。
      
        - gwaddr：
          自定义后端 API 系统的网络地址，如：192.168.1.2:8080。
          如果 gwurl 中带有域名，则填写了该配置后，可以不查询 DNS。

        - method：
          向后端 API 系统发起请求时要使用的 HTTP 方法，合法值为：`GET`、`POST` 或空。
          不填写则默认为空，即保留原始的 HTTP 请求方法。

        - app_id：
          向后端 API 系统发起请求时要使用的 `api_appid` 参数。

        - sign_key：
          向后端 API 系统发起请求时要使用的签名密钥，不填写则不对请求进行签名。

        - nonce_length：
          向后端 API 系统发起请求时要带上的 `api_nonce` 随机串的长度，为 0 则表示不带随机串。
          不填写则默认为 0。
 
        - default_params：
          向后端 API 系统发起请求时使用的默认请求参数值，当请求参数不存在时，将使用该配置中定义的参数值。

        - override_params：
          向后端 API 系统发起请求时要覆盖的请求参数值。
          此处的参数值若以 `@` 开头，则表示读取 session 中的值作为参数值，格式如：`@session名称.session值键名`。
          若配置了从 session 中读取参数值，而请求时 session 中没有该值，则将忽略该参数值。

        - delete_params：
          向后端 API 系统发起请求时要删除的请求参数。

        - match_params：
          参数匹配条件，只有匹配了此处的参数值，请求才会代理到相应的后端 API 系统中。
          支持对多个请求参数值进行匹配，参数值支持通配符 `*`。

      
配置示例：

	apiproxy:
	  backends:
	    myapi:
	      gwurl: http://api.myapi:8080/
	      gwaddr: 192.168.1.2:8080
	      method: GET
	      sign_key: "123456"
	      nonce_length: 16
	      default_params:
	        api_debug: "1"
	      override_params:
	        UserId: "@default.user_id"
	      delete_params:
	      - api_server
	      match_params:
	        api_action:
	        - Action.Name

### 相关错误

| 错误码                     | 错误消息                     | 错误描述            |
| ------------------------- | --------------------------- | ------------------ |
| GatewayFailed             | Gateway request failed!     | 请求网关失败！       |
| GatewayResultUnrecognized | Gateway result unrecognized!| 网关返回结果不可识别！|
| SessionNotReady           | Session is not ready!       | 会话未准备好！       |

###接口参考：

- `func (p *Proxy) GetClient(alias string) *apiclient.Client`
  获取用于调用 apiproxy 后端的 client。

- `func (p *Proxy) ProxyCall(c *api.Context, backendAlias string, action string, params url.Values) (*apiclient.Response, *api.Error)`
  根据 apiproxy 配置进行代理请求。

- `func (p *Proxy) AddBackend(alias string, backend *Backend)`
  增加后端代理，Backend格式为：

		type Backend struct {
			GWURL            string
			GWADDR           string
			Method           string
			AppId            string
			SignKey          string
			NonceLength      int
			DefaultParams    map[string]string
			OverrideParams   map[string]string
			DeleteParams     []string
			MatchParams      map[string][]string
		}

- `func (p *Proxy) DeleteBackend(alias string)`
  删除指定后端代理。

- `func (s *Sign) GetSignKey() string`
  返回签名密钥。

- `func (s *Sign) SetSignKey(signKey string)`
  设置签名密钥。

- `func (p *Proxy) Enable()`
  启用该中间件。

- `func (p *Proxy) Disable()`
  禁用该中间件。

###使用示例：

	package main
	
	import (
		"os"
	
		"git.quyun.com/apibox/api"
		"git.quyun.com/apibox/apiproxy"
	)
	
	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apiproxy.NewProxy(app))
		app.Run(apiRoutes)
	}
