请求上下文
===========

Context（上下文）是在 HTTP 请求过程中保留各种资源的环境，在 APIBox 中，Context 以参数的形式传入 ActionFunc：

	type ActionFunc func(c *Context) (data interface{})

Context 结构如下：

    type Context struct {
    	App    *App
    	Input  *Input
    	Output *Output
    	DB     *DbManager
    	Model  *ModelManager
    	Error  *ErrorManager
    }

结构中各个成员分别表示：

 - `App`：应用
 - `Input`：HTTP 输入
 - `Output`：HTTP 输出
 - `DB`：数据源
 - `Model`：模型
 - `Error`：API 错误

## 接口及成员

 - `func (c *Context) Request() *http.Request`
    返回当前请求。

 - `func (c *Context) Response() http.ResponseWriter`
    返回当前响应

 - `func (c *Context) Session(sessionName string) (*sessions.Session, error)`
   返回指定名称的 Session。

### 环境变量管理

Context 支持通过以下接口对请求上下文的环境变量进行管理：

	func (c *Context) GetDB(key string) *xorm.Engine
	func (c *Context) Set(key, val interface{})
	func (c *Context) Delete(key interface{})
	func (c *Context) Get(key interface{}) interface{}
	func (c *Context) GetOk(key interface{}) (interface{}, bool)
	func (c *Context) GetAll() map[interface{}]interface{}
	func (c *Context) GetAllOk() (map[interface{}]interface{}, bool)

APIBox会识别以下特殊环境变量：

 - `response_closed`：bool类型，当设置为true时，将不会输出API结果。

## App

应用表示当前应用，结构如下：

	type App struct {
		Name          string
		Config        *Config
		DB            *DbManager
		Model         *ModelManager
		Error         *ErrorManager
		Logger        *Logger
	}

可以在 Action 中通过 `c.App.Config` 提供的函数读取应用配置，如：

	func AppNameAction(c *api.Context) interface{} {
		return c.App.Config.GetDefaultString("app.name", "unknown")
	}

相关配置项和配置相关函数可参考：

 - [配置](config.md)


## Input

Input 对 `http.Request` 进行了封装，结构如下：

	type Input struct {
		Request *http.Request
	}

通过 `c.Input.Request` 可以直接使用 `http.Request` 提供的函数。

Input 自身封装了以下几个函数：

 - `func (i *Input) Get(key string) string`

   获取请求参数值，如果同名参数有多个值，则只返回第一个值。

 - `func (i *Input) GetDefault(key string, defaultValue string) string`

   获取请求值，如果请求值为空，则返回 `defaultValue` 值。

 - `func (i *Input) GetAction() string`

    获取 action 参数。
 
 - `func (i *Input) GetAll() map[string]string`

    获取所有请求参数，如果同名参数有多个值，则只返回第一个值。

 - `func (i *Input) GetForm() url.Values`
 
    获取原始表单参数值。

 - `func (i *Input) Has(key string) bool`

    检测请求参数是否存在，只有当请求参数中不出现该参数时才返回 false。

    示例URL：http://example.com/?api_action=test&param1=&param2=bar
    - `c.Input.Has("param1")`: true
    - `c.Input.Has("param2")`: true
    - `c.Input.Has("param3")`: false


## Output

Output 对 `http.ResponseWriter` 进行了封装，结构如下：

	type Output struct {
		ResponseWriter http.ResponseWriter
	}

通过 `c.Output.ResponseWriter` 可以直接使用 `http.ResponseWriter` 提供的函数。

Output 自身封装了以下几个函数：

 - `func (o *Output) Write(data []byte) (int, error)`
	
	输出响应数据。该方法在 Action 中一般不会用到。

 - `func (o *Output) SetHeader(key, value string)`

    设置 HTTP 响应头。

## DB

DB 成员中维护着应用的数据源，可以通过它获取到应用配置文件中定义的数据源：

 - `func (dbm *DbManager) GetMysql(dbAlias string) (*xorm.Engine, error)`
   返回 Mysql 数据源

 - `func (dbm *DbManager) GetSqlite3(dbAlias string) (*xorm.Engine, error)`
   返回 Sqlite3 数据源

更多关于数据源的信息，请参考：

 - [数据源](database.md)

## Model

Model 成员中维护着应用的数据模型，相关接口：

 - `func (mm *ModelManager) Register(model interface{})`
   注册模型，只有注册过的模型才能使用 dbop 快捷操作。

 - `func (mm *ModelManager) Unregister(modelName string)`
   移除模型。

 - `func (mm *ModelManager) Has(modelName string) bool`
   检查模型是否存在。

 - `func (mm *ModelManager) Get(modelName string) *Model`
   获取指定模型。
   
## Error

Context 中 Error 成员的唯一作用是新建一个 API 错误，该错误一般用作 Action 的返回值，如：

	func TestErrorAction(c *api.Context) interface{} {
		return c.Error.New(api.ErrorInvalidParam, "Password", "TooShort")
	}

更多关于 API 错误的信息，请参考：

 - [错误](error.md)