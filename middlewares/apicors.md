API CORS跨域请求
=================

### 相关配置

- apicors：
   API CSRF 防护中间件配置组

  - disabled:
    是否禁用该中间件，不配置则默认为：`false`。

  - `allow_origins`:
    允许跨域请求的来源列表，默认为 `*`。

  - `allow_credentials`:
    是否允许使用凭据（即跨域使用Cookie），默认为否。

  - `max_age`:
    CORS 策略缓存时间，单位为秒，默认为 -1，表示不缓存。

  - actions：
      接口匹配规则。

     - whitelist：
        接口白名单：要进行 CORS 策略的接口名称列表，默认会对所有接口应用 CORS 策略。
        支持通配符 `*`。

     - blacklist：
        接口黑名单：要忽略 CORS 策略的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行 CSRF 检测。


配置示例：

	apicors:
	  allow_origins:
	  - '*'
	  allow_credentials: true
	  max-age: 600
	  actions:
	    blacklist:
	    - Attachment.Download
