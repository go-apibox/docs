应用
=====

## 创建应用

可通过以下两个函数来创建应用

 - `func NewApp() (*App, error)`
   将会读取 `config/app.yaml` 作为应用的配置文件。

 - `func NewAppFromYaml(yamlStr string) (*App, error)`
    不使用配置文件，直接使用参数 `yamlStr` 指定的配置。

## 运行应用

通过以下函数运行应用：

    func (app *App) Run(routes []Route)

运行步骤：
1. 读取配置文件
2. 根据配置文件中的 `app.name` 初始化日志服务
3. 根据配置文件中的 `api.default.lang` 设置API默认语言
4. 根据配置文件中的 `words` 段加载词汇转换规则
5. 根据配置文件中的 `mysql` 段和 `sqlite3` 加载数据库配置
6. 注册中间件（可选：如签名验证、重攻击防护）
7. 根据 routes 生成路由并注册为 http 处理函数
8. 根据配置文件中的 `app.http_addr` 监听并启动 HTTP 服务

## 读取配置

应用的结构如下：

	type App struct {
		Name          string
		Host          string
		Addr          string
		Router        *mux.Router
		Config        *Config
		DB            *DbManager
		Model         *ModelManager
		Error         *ErrorManager
		Logger        *Logger
		Hooks         map[string]ActionFunc
	}

后续可以通过应用结构中的成员 `Config` 来读取配置项，如：

	app.Config.GetString("app.name")

具体请参考：

 - [配置](config.md)

## 获取数据源

应用结构中的成员 `DB` 为数据源管理器，可以通过它来获取在应用配置中定义的数据源，如：

	app.DB.GetMysql("default")

具体请参考：

- [数据库](database.md)

## 使用中间件

如果需要在应用中加入中间件，可以使用以下方法：

	func (app *App) Use(middlewares ...negroni.Handler)

中间件规范请查看 negroni 相关文档：

  - [https://github.com/codegangsta/negroni](https://github.com/codegangsta/negroni)

## 处理原始输入输出

可以通过以下方法增加对原始输入输出数据的处理：

	type RawIOHandler func(reqId string, rawReq, rawResp []byte)
	func (app *App) AddRawIOHandler(h RawIOHandler)
	
一般用于日志记录。

## 钩子Action

可对应用配置以下全局的钩子 Action：

 - `BeforeAction`：在所有 Action 执行前执行，如返回非 nil，则不再执行后续 Action。
 - `AfterAction`：在所有 Action 执行后执行，如主 Action 返回值非 nil，钩子 Action 的返回值将被忽略。上一步 Action 的执行结果会保存在名为 `output` 的 context 变量中，在钩子 Action 中可以通过 `c.Get("output")` 来获取。

可通过以下接口添加全局钩子 Action：

 - `func (app *App) Hook(tag string, actionFunc ActionFunc) *App`
   参数 tag 可取值为：`BeforeAction`、`AfterAction`

示例（将 default mysql 数据源注入 context，如果错误则不再往下执行 Action）：

	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(-1)
		}
	
		app.Hook("BeforeAction", injectDB)
		app.Run(apiRoutes)
	}
	
	func injectDB(c *api.Context) (data interface{}) {
		db, err := c.DB.GetMysql("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "DBNotExist").SetMessage(err.Error())
		}
	
		db.ShowSQL = c.App.Config.GetDefaultBool("xorm.show_sql", false)
		db.ShowDebug = c.App.Config.GetDefaultBool("xorm.show_debug", false)
		db.ShowErr = c.App.Config.GetDefaultBool("xorm.show_error", false)
		db.ShowInfo = c.App.Config.GetDefaultBool("xorm.show_info", false)
		db.ShowWarn = c.App.Config.GetDefaultBool("xorm.show_warn", false)
		
		c.Set("db", db)
		return nil
	}

## Session 存储

以下接口将根据应用配置文件中的 `session` 段的配置，初始化返回应用的 Session 存储（单例）：

 - `func (app *App) SessionStore() (*session.CookieStore, error)`
