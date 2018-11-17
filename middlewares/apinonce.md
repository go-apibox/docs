API重攻击防护
=============

### 实现原理

所谓网络重攻击，就是使用同一个 URL 地址多次调用同一个 API，达成对目标系统的攻击。防御网络重攻击的目标，就是要保证同一个 API 的 URL 地址只能被调用一次。

我们通过在请求参数中引入一个随机串来实现对网络重攻击的防御，每次请求中的随机串都会被缓存在服务端，当重复使用同一个随机串进行 API 调用时，就会返回失败。

因为历史随机串会定期被系统清理，在历史随机串被清理后，如果再次调用同一个 API 的 URL 地址，那么仍然会调用成功。因此随机串一般与时间戳配合使用，在 URL 请求过期后才开始清理历史随机串。

### 相关参数

 - api_nonce
    签名随机串，用于防止网络重攻击。

### 相关配置

 - apinonce：
    API重攻击防护中间件配置组

   - disabled:
     是否禁用该中间件，不配置则默认为：`false`。

   - length：
      随机串长度，默认值为：`16`。
	
   - expire_time：
     缓存的 nonce 过期时间，单位为秒，默认值为：`1000`。
     如果该中间件与 apisign 共同使用，一般设置该值大于 `api.sign.expire_time`，以确保签名过期后才清理缓存。
     该中间件会每隔 `expire_time` 清理一次过期的缓存。

   - max_cache_count：
      最大缓存的 nonce 条数，默认值为：`100000`。
      如果达到最大缓存条数，则返回 API 错误。

   - actions：
     接口匹配规则。

     - whitelist：
       接口白名单：要进行重攻击防护的接口名称列表，默认会对所有接口进行重攻击防护。
       支持通配符 `*`。

     - blacklist：
       接口黑名单：要忽略重攻击防护的接口名称列表，默认为空，即不忽略所有接口。
       支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行重攻击防护。

配置示例：

	apinonce:
	  length: 16
	  expire_time: 1000
	  max_cache_count: 100000
	  actions:
	    blacklist:
	    - Public.Action1
	    - Public.Action2


### 相关错误

| 错误码                 | 错误消息                         | 错误描述       |
| --------------------- | ------------------------------- | ------------- |
| MissingNonce          | Missing nonce!                  | 缺少随机串！    |
| InvalidNonce          | Invalid nonce!                  | 无效的随机串！  |
| NonceExist            | Signature nonce already exists! | 随机串已存在！  |
| NonceCountExceed      | Nonce count exceed!             | 随机串数量超出限制！  |

### 接口参考

- `func (n *Nonce) Enable()`
  启用该中间件。

- `func (n *Nonce) Disable()`
  禁用该中间件。

### 使用示例

    package main
    
    import (
    	"os"
    
    	"github.com/go-apibox/api"
    	"github.com/go-apibox/apinonce"
    	"github.com/go-apibox/apisign"
    )
    
    func main() {
    	app, err := api.NewApp()
    	if err != nil {
    		println(err.Error())
    		os.Exit(1)
    	}
    
    	app.Use(apinonce.NewNonce(app))
    	app.Use(apisign.NewSign(app))
    	app.Run(apiRoutes)
    }

