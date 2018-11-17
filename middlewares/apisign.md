API签名验证
============

### 实现原理

API调用签名机制，是验证调用者身份的一种手段，是通过对请求的 URL 数据进行签名实现的。开启签名机制后，只有带有签名串参数的 API 调用才是合法的。

为加强安全性，APIBox 实现了签名过期机制，签名后的 API 的 URL 地址只能在一定时间内访问才有效，当签名过期后，调用 API 的 URL 地址将会返回错误。

另外，APIBox 还支持多个应用使用不同的签名密钥进行签名，应用签名密钥保存在数据库中。

签名验证算法统一采用 hmac_md5。

### 相关参数

 - api_timestamp
   请求时间戳

 - api_sign
   签名串

### 相关配置

- apisign：
    API签名验证中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。

    - sign_key：
	  签名使用的key，为空则不进行验证。

    - app:
      是否启用应用，不同的应用使用不同的签名key。

        - enabled:
          是否启用。如果配置了该项，则`sign_key`无效，不配置则默认为：`false`。

        - db_type：
          数据库引擎类型，如：mysql、sqlite3，不填则默认为 `mysql`。

        - db_alias：
          数据源名称，不填则默认为：`default`。

        - table:
          签名使用的key来自的数据表名，不配置则默认为：`app`。

        - app_id_column:
          对应app_id的字段名，不配置则默认为：`app_id`。

        - app_id_type:
          对应app_id的字段类型，可选 string 或 int，不配置则默认为：`int`。

        - sign_key_column:
          对应sign_key的字段名，不配置则默认为：`sign_key`。

        - app_status_column:
          对应应用状态的字段名，不配置则默认为：`status`。
          应用状态必须为 `normal` 才正常。

        - admin_app_enabled:
          是否启用管理应用，不配置则默认为：`false`。

        - admin_app_id:
          管理应用ID，不配置则默认为：`admin`。

        - admin_sign_key:
          管理应用签名key，为空或不配置则不进行签名验证。

    - expire_time：
      签名过期时间，单位为秒，默认值为：`600`。
      
    - allow_time_offset：
      最大允许的签名时间偏差，单位为秒，默认值为：`300`。

    - actions：
      接口匹配规则。

      - whitelist：
        接口白名单：要进行签名验证的接口名称列表，默认会对所有接口进行签名验证。
        支持通配符 `*`。

      - blacklist：
        接口黑名单：要忽略签名验证的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行签名验证。
      
配置示例：

	apisign:
	  sign_key: abcdef
	  expire_time: 600
	  allow_time_offset: 300
	  actions:
	    blacklist:
        - Public.Action1
        - Public.Action2

### 相关错误

| 错误码                 | 错误消息                         | 错误描述         |
| --------------------- | ------------------------------- | --------------- |
| AppNotExist           | Application does not exist!     | 应用不存在！      |
| MissingSign           | Missing signature!              | 缺少签名！        |
| SignError             | API signature check failed!     | 签名校验失败！    |
| MissingTimestamp      | Missing timestamp!              | 缺少时间戳！      |
| InvalidTimestamp      | Timestamp is invalid!           | 时间戳不合法！    |
| SignExpired           | Request has expired!            | 请求已超时！      |

### 接口参考

- `func (s *Sign) Enable()`
  启用该中间件。

- `func (s *Sign) Disable()`
  禁用该中间件。

###使用示例：

	package main
	
	import (
		"os"
	
		"git.quyun.com/apibox/api"
		"git.quyun.com/apibox/apisign"
	)
	
	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apisign.NewSign(app))
		app.Run(apiRoutes)
	}