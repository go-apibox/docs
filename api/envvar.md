环境变量
=========

APIBox 会对以下环境变量进行处理：

- APP_HOST：
  应用绑定的域名，对应配置：app.host

- APP_ADDR：
  应用监听的地址，对应配置：app.http_addr

- APP_DB：
  应用使用的数据库，仅支持sqlite3。
  示例：APP_DB="default:test.db;log:log.db"

- APP_ALLOW_WILD：
  是否允许应用变为野进程，对应配置：app.allow_wild

- DEBUG_LEVEL：
  调试等级，启用后会在API调用结束后输出调用结果。
  可选值为：code（仅打印API返回代码）、full（打印完整的API结果）
