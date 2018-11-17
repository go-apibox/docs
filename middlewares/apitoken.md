API幂等性
============

### 实现原理

当调用 API 接口执行某些操作时，如果遇到了请求超时或服务器内部错误，客户端可能会尝试重发请求，这时客户端可以通过提供可选参数 `api_token` 避免服务器执行重复的操作，也就是通过提供 `api_token` 参数保证请求的幂等性。

`api_token` 是一个由客户端生成的唯一的、大小写敏感的字符串。

如果用户使用同一个 `api_token` 值调用创建实例接口， 则服务端会返回相同的请求结果。因此用户在遇到错误进行重试的时候，可以通过提供相同的 `api_token` 值，来确保操作只执行一次。

如果用户提供了一个已经使用过的 `api_token`，但其他请求参数不同，则服务端会返回 `ParamMismatch` 的错误代码。 但需要注意的是， 中间件仅会检测业务参数 和 `api_appid`，其它参数在重试时是可以变化的，如 API 签名中间件使用的 `api_sign`、`api_timestamp`。

### 相关参数

 - api_token
   API Token，由客户端创建的随机字符串。

### 数据表结构

如果以下表不存在，则中间件会尝试自动创建表。

#### MySQL表：

	CREATE TABLE IF NOT EXISTS `api_token` (
	  `token_id` VARCHAR(255) COLLATE utf8_bin NOT NULL COMMENT 'API TOKEN',
	  `action` VARCHAR(255) NOT NULL COMMENT '接口名称',
	  `request_time` INT UNSIGNED NOT NULL COMMENT '请求时间',
	  `params` TEXT NOT NULL COMMENT '业务参数，URLENCODE格式',
	  `params_md5` VARCHAR(255) NOT NULL COMMENT '业务参数MD5值',
	  `response_body` LONGTEXT NOT NULL COMMENT '响应内容，不含HEADER，json格式',
	  PRIMARY KEY (`token_id`))
	ENGINE = InnoDB DEFAULT CHARSET=utf8

#### Sqlite3表：

	CREATE TABLE IF NOT EXISTS `api_token` (
		`token_id`	TEXT NOT NULL PRIMARY KEY, -- API TOKEN
		`action`	TEXT NOT NULL, -- 接口名称
		`request_time`	INTEGER NOT NULL, -- 请求时间
		`params`	TEXT NOT NULL, -- 业务参数，URLENCODE格式
		`params_md5`	TEXT NOT NULL, -- 业务参数MD5值
		`response_body`	TEXT NOT NULL -- 响应内容，不含HEADER，json格式
	);

### 相关配置

 - apitoken：
      API幂等性处理中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。

    - db_type：
      数据库类型，支持 `mysql` 和 `sqlite3`，默认设置为：`mysql`。

    - db_alias：
      数据源别名，默认设置为：`default`。

    - table_name：
      token 表名，默认设置为：`api_token`。

    - actions：
      接口匹配规则。

      - whitelist：
        接口白名单：要进行幂等性处理的接口名称列表，默认会对所有接口进行幂等性处理。
        支持通配符 `*`。

      - blacklist：
        接口黑名单：要忽略幂等性处理的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行幂等性处理。
      
配置示例：

	apitoken:
	  db_type: mysql
	  db_alias: default
	  table_name: api_token
	  actions:
	    whitelist:
        - Server.Create
        - Account.Deposit

### 相关错误

| 错误码                 | 错误消息                         | 错误描述                  |
| --------------------- | ------------------------------- | ----------------------- |
| MissingToken          | Missing API token!              | 缺少 API Token！         |
| InvalidToken          | API token is invalid!           | API token不合法！        |
| ParamMismatch         | Param mismatch with API token!  | 参数与 API Token 不匹配！ |

### 接口参考

- `func (t *Token) Enable()`
  启用该中间件。

- `func (t *Token) Disable()`
  禁用该中间件。

###使用示例：

	package main
	
	import (
		"os"
	
		"github.com/go-apibox/api"
		"github.com/go-apibox/apitoken"
	)
	
	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apitoken.NewToken(app))
		app.Run(apiRoutes)
	}