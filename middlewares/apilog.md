API日志记录
===========

### 实现原理

日志记录中间件会将 API 请求记录到数据库中。

支持记录原始请求响应内容，如：

请求：

	GET /?api_action=Test.Param&User={%22Username%22:%22hilyjiang%22} HTTP/1.1
	Host: 192.168.122.120:8080
	User-Agent: Go 1.1 package http
	Connection: close
	Accept-Encoding: gzip

响应：

	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	Date: Wed, 31 Dec 2014 22:31:48 GMT
	Content-Length: 101
	Connection: close
	X-Request-Id: cebce6d1-913c-11e4-8e3a-ec4018c68cc7
	
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": {
	        "Username": "hilyjiang"
	    }
	}

### 数据表结构

如果以下表不存在，则中间件会尝试自动创建表。

#### MySQL日志表：

	CREATE TABLE IF NOT EXISTS `api_log` (
	  `log_id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
	  `request_id` VARCHAR(255) NOT NULL COMMENT '请求ID',
	  `request_method` VARCHAR(10) NOT NULL COMMENT 'HTTP请求类型',
	  `app_id` VARCHAR(255) NOT NULL COMMENT '调用方使用的应用ID',
	  `action` VARCHAR(255) NOT NULL COMMENT '接口名称',
	  `code` VARCHAR(255) NOT NULL COMMENT '调用返回代码',
	  `ip` VARCHAR(255) NOT NULL COMMENT '调用方的IP地址',
	  `country` VARCHAR(255) NOT NULL COMMENT '调用方所在国家',
	  `region` VARCHAR(255) NOT NULL COMMENT '调用方所在地区',
	  `city` VARCHAR(255) NOT NULL COMMENT '调用方所在城市',
	  `isp` VARCHAR(255) NOT NULL COMMENT '调用方的ISP服务商',
	  `request_time` INT UNSIGNED NOT NULL COMMENT '请求时间戳',
	  `elapse_time` BIGINT UNSIGNED NOT NULL COMMENT '消耗时间，单位为纳秒',
	  PRIMARY KEY (`log_id`),
	  INDEX `idx_request_id` (`request_id` ASC),
	  INDEX `idx_app_id` (`app_id` ASC),
	  INDEX `idx_action` (`action` ASC),
	  INDEX `idx_ip` (`ip` ASC))
	ENGINE = InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1

#### MySQL 日志详情表：

	CREATE TABLE IF NOT EXISTS `api_log_detail` (
	  `log_id` BIGINT UNSIGNED NOT NULL,
	  `request_id` VARCHAR(255) NOT NULL COMMENT '请求ID',
	  `request` LONGTEXT NOT NULL COMMENT '请求原文',
	  `response` LONGTEXT NOT NULL COMMENT '响应原文',
	  PRIMARY KEY (`log_id`),
	  INDEX `idx_request_id` (`request_id` ASC))
	ENGINE = InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1

#### SQLite3 日志表：

	CREATE TABLE IF NOT EXISTS `api_log` (
		`log_id`	INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
		`request_id`	TEXT NOT NULL, -- 请求ID
		`request_method`	TEXT NOT NULL, -- HTTP请求类型
		`app_id`	TEXT NOT NULL, -- 调用方使用的应用ID
		`action`	TEXT NOT NULL, -- 接口名称
		`code`	TEXT NOT NULL, -- 调用返回代码
		`ip`	TEXT NOT NULL, -- 调用方的IP地址
		`country`	TEXT NOT NULL, -- 调用方所在国家
		`region`	TEXT NOT NULL, -- 调用方所在地区
		`city`	TEXT NOT NULL, -- 调用方所在城市
		`isp`	TEXT NOT NULL, -- 调用方的ISP服务商
		`request_time`	INTEGER NOT NULL, -- 请求时间戳
		`elapse_time`	INTEGER NOT NULL -- 消耗时间，单位为纳秒
	);

#### SQLite3 日志详情表：

	CREATE TABLE IF NOT EXISTS `api_log_detail` (
		`log_id`	INTEGER NOT NULL,
		`request_id`	TEXT NOT NULL, -- 请求ID
		`request`	TEXT NOT NULL, -- 请求原文
		`response`	TEXT NOT NULL, -- 响应原文
		PRIMARY KEY(log_id)
	);

### 相关配置

- apilog：
  API日志记录中间件配置组

  - disabled：
    是否禁用该中间件，不配置则默认为：`false`。

  - db_type：
    数据库类型，支持 `mysql` 和 `sqlite3`，默认设置为：`mysql`。

  - db_alias：
    数据源别名，默认设置为：`default`。

  - table：
    日志表名配置。

     - log：日志表名，默认设置为：`api_log`，为空表示不记录日志。
     - detail：详情请求表名，默认设置为：`api_log_detail`，为空表示不记录详情日志。
 
  - geo_enabled：
	是否根据IP查询地理位置并写入日志，默认设置为：`true`。

  - actions：
    接口匹配规则。

     - whitelist：
        接口白名单：要进行日志记录的接口名称列表，默认会对所有接口进行日志记录。
        支持通配符 `*`。

     - blacklist：
        接口黑名单：要忽略日志记录的接口名称列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。
         
  - codes：
    API返回代码匹配规则。
      
     - whitelist：
        接口白名单：要进行日志记录的返回代码列表，默认会对除 `ActionNotExist` 外的所有返回代码进行日志记录。
        支持通配符 `*`。

     - blacklist：
        接口黑名单：要忽略日志记录的返回代码列表，默认列表为：`ActionNotExist`。
        支持通配符 `*`。

  - replace_rules:
    数据替换规则列表，map格式。规则必须为正则表达式。
    如：`(?i)(?:Password=)[^&\s]+`: "Password=******"

该中间件会先检查接口是否在 `whitelist` 列表中，如果在，则再验证接口是否被  `blacklist` 列表排除。
只有匹配了 `whitelist` 列表，且未被 `blacklist` 排除的接口才会进行日志记录。

配置示例：

	apilog:
	  db_type: mysql
	  db_alias: default
	  table:
	    log: api_log
	    detail: api_log_detail
	  geo_enabled: true
	  actions:
	    blacklist:
	    - Public.Action1
	    - Public.Action2
	  codes:
	    blacklist:
	    - ActionNotExist

### 接口参考

- `func (l *Log) Enable()`
  启用该中间件。

- `func (l *Log) Disable()`
  禁用该中间件。

### 使用示例

    package main
    
    import (
    	"os"
    
    	"git.quyun.com/apibox/api"
    	"git.quyun.com/apibox/apilog"
    )
    
    func main() {
    	app, err := api.NewApp()
    	if err != nil {
    		println(err.Error())
    		os.Exit(1)
    	}
    
    	app.Use(apilog.NewLog(app))
    	app.Run(apiRoutes)
    }