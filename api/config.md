配置
=====

APIBox 使用最为灵活的 YAML 作为配置信息的格式。


## 载入配置

在创建应用时，有两种载入配置的方式：

1. 配置文件
 
    当调用 `api.NewApp()` 时，默认将读取 `config/app.yaml` 作为配置文件。
    这种方式加载的配置文件，支持 `include` 配置（引入其它配置文件）。

2. 配置字符串

   直接将配置信息以 YAML 格式传递给 `api.NewAppWithYaml()`。
   这种方式加载的配置文件，不支持 `include` 配置（引入其它配置文件）。


## 读取配置

在 Action 中可以通过 `c.App.Config` 访问到应用配置对象，配置对象的接口接口可参考：

 - [apibox/config 文档](../other/config.md)

读取配置时，多级配置名可以用 `.` 号分隔，例如：

 - `app.name`：应用名称
 - `app.http_addr`：应用监听的地址


## 配置项

以下是 APIBox 提供的核心配置项，配置项可自定义作为扩展。

- `app`：
  应用配置

  - `name`：
    应用名称，将会在 log 中输出，不填则默认为 `app`

  - `host`：
    应用绑定的域名。

  - `http_addr`：
    应用监听的地址，格式为：`ip:port`，不填则默认为`:80`

  - `tls`：
    TLS相关配置段

    - `enabled`：
      是否启用 tls（https），不填则默认为 `false`

    - `cert`：
      证书文件路径，不填则默认为 `server.crt`。
      如果证书文件不存在，则将使用自动生成的证书。

    - `key`：
      私钥文件路径，不填则默认为 `server.key`
      如果私钥文件不存在，则将使用自动生成的私钥。

  - `allow_wild`：
    是否禁止应用运行为pid=1（根进程）的子进程，默认为false。
    如果有传入环境变量`APP_ALLOW_WILD`（可选值为0或1），则以环境变量设置为准。

- `api`：
  API 相关配置

  - `path`：
    API 请求路径，默认设置值为 `/`。

  - `default`：
    请求参数默认值设置

    - `format`：
      设置 `api_format` 的默认值。
      API 返回结果的默认格式，可选值：`json`、`jsonp`，默认设置值为 `json`。

    - `callback`：
      设置 `api_callback` 的默认值。
      API 返回格式为 `jsonp` 时，作为 javascript 回调函数名，默认设置值为 `callback`。

    - `lang`：
      设置 `api_lang` 的默认值。
      应用默认语言（用于错误消息输出），格式如：`en_us`、`zh_cn`。
      不设置则默认设置值为 `en_us`。
    
    - `debug`：
      设置 `api_debug` 的默认值。
      是否启用 API 调试，启用后返回数据格式将会自动美化。
      可选值为：`true`、`false`，默认设置值为 `false`。

  - `log`：
    请求日志设置。

    - `enabled`：
      是否输出请求日志，不配置则默认为：`true`。
  
    - `actions`：
      接口匹配规则。

      - `whitelist`：
        接口白名单：要输出请求日志的接口名列表，默认会输出所有接口的请求日志。
        支持通配符 `*`。

      - `blacklist`：
        接口黑名单：不输出请求日志的接口名列表，默认为空，即不忽略所有接口。
        支持通配符 `*`。

  - `allow_formats`：
    允许的 API 返回结果的格式，不填则默认只允许 `json` 格式（jsonp有一定的安全风险）。
    配置该项后，如果 `api_format` 值不在列表中，则默认以列表第一个值指定的格式返回 API 结果。

- `session`：
  Session类库配置段。
    
  - `store_type`:
    密钥保存介质，可为 `file` 或 `memory`。如果未配置则默认为 `memory`。

  - `key_pairs_file`：
    密钥文件，如果未配置，则默认密钥文件为：`{app.name}_session.key`（{app.name}会自动替换为应用名称）。

- `mysql`：
  mysql数据源设置，支持多个数据源

  - `enabled`：
    是否启用该数据源，不填则默认为 `true`

  - `protocol`：
    mysql 连接协议，默认为 `tcp`
    更多可设置值见：
    [http://golang.org/pkg/net/#Dial](http://golang.org/pkg/net/#Dial)

  - `address`：
    mysql服务器连接地址，不填则默认为 `127.0.0.1:3306`
    
  - `dbname`：
    数据库名称

  - `user`：
    mysql数据库的用户名

  - `passwd`：
    mysql数据库的密码

  - `max_idle_conns`：
   设置连接池的空闲数大小，不填则默认为 `5`。

  - `max_open_conns`：
   设置最大打开连接数，不填则默认为 `100`。

  - `show_sql`：
   是否打印SQL语句。

  - `log_level`：
   日志等级：error、warning、info、debug、off。

- `sqlite3`：
  sqlite3数据源设置，支持多个数据源

  - `enabled`：
    是否启用该数据源，不填则默认为`true`

  - `db`：
    sqlite3数据库文件路径

  - `allow_env`：
    是否允许使用环境变量中的APP_DB设置

  - `show_sql`：
   是否打印SQL语句。

  - `log_level`：
   日志等级：error、warning、info、debug、off。

- `dbop`：
  数据库快速操作（List/Detail/Create/Update等）的配置。

  - `db_type`：
    数据库引擎类型，如：`mysql`、`sqlite3`，不填则默认为 `mysql`。

  - `db_alias`：
    数据源名称，不填则默认为：`default`。

- `words`：
  词汇转换规则配置，格式可见示例。

- `include`:
  要引入的其它配置文件列表，不需要加.yaml后缀，引入的配置文件将会覆盖当前文件中的配置。

> 提示：
> 
>  如果在配置中使用了 mysql 或 sqlite3，那么必须在应用入口文件中加载相应的数据库驱动，否则配置将被忽略。
>
> mysql 驱动：
> 
>     import (
>     	_ "github.com/go-sql-driver/mysql"
>     )
> 
> sqlite3 驱动：
> 
>     import (
>     	_ "github.com/mattn/go-sqlite3"
>     )


## 示例配置

    # config of application
    app:
      name: myapi
      http_addr: :8080
    
    # config of api
	api:
      default:
        format: json
        callback: callback
        lang: zh_cn
        debug: true
    
	# session config
	session:
    store_type: file
    key_pairs_file: myapi_session.key
    
    # mysql config
    mysql:
      default:
        enabled: true
        address: 192.168.1.2:3306
        dbname: test
        user: test
        passwd: test
      slave:
        enabled: true
        address: 192.168.1.3:3306
        dbname: test
        user: test
        passwd: test
    
    # sqlite3 config
    sqlite3:
      default:
        enabled: true
        db: db.sqlite
    
	# dbop config
	dbop:
    db_type: mysql
    db_alias: default
    
    # words
    words:
      en_us:
        Username: username
        Password: password
    zh_cn:
    	Username: 用户名
    	Password: 密码
    
    # include extend config
    include:
    - dev
    