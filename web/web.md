Web 类库
========

Web 类库帮助你快速创建基于配置规则的 Web 服务器。

一般的 Web 服务可以响应三种类型的请求，三种请求分别对应不同的处理方式：

一、静态文件
    直接返回静态文件内容，特性：
    1. 可支持设置 `max-age` 缓存选项
    2. 对于 `.eot`、`.ttf`、`.woff` 结尾的文件，自动设置 cors 头：`Access-Control-Allow-Origin: *`

二、模板页面
    使用 pongo2 模板语法对页面进行渲染后输出，特性：
    1. 支持按访问路径匹配模板文件
    2. 支持向模板中注入数据
    3. 支持页面访问权限检测

三、API请求
    Web 服务器本身不实现 API 接口，API 请求会全部转发到后端的 API 服务器上。特性：
    1. 支持多个后端 API 服务器
    2. 支持设置默认请求参数
    3. 支持从 API 服务器同步 session


### Web 服务定义

结构：

	type Web struct {
		webRoot string
		host    string
	
		pages   []*Page
		apis    []*API
		statics []*Static
	
		sessionStores map[string]*session.CookieStore // 支持不同的session使用不同的cookiestore
	
		injectData     map[string]interface{}
		injectSessions map[string]string
	
		debug bool
	}

方法：

 - `func New(webRoot, host string) (*Web, error)`
	新建空的 Web 服务定义。

 - `func FromConfig(webRoot, host string, config *WebConfig) (*Web, error)`
   从配置对象中创建 Web 服务定义，该方法将自动输入配置对象中的所有选项。

 - `func (web *Web) AddPage(page *Page) *Web`
   添加单个模板页面定义。

 - `func (web *Web) AddPages(pages []*Page) *Web`
   添加多个模板页面定义。

 - `func (web *Web) AddAPI(api *API) *Web`
   添加单个 API 定义。

 - `func (web *Web) AddAPIs(apis []*API) *Web`
   添加多个 API 定义。

 - `func (web *Web) AddStatic(static *Static) *Web`
   添加单个静态文件定义。
 
 - `func (web *Web) AddStatics(statics []*Static) *Web`
   添加多个静态文件定义。
 
 - `func (web *Web) PageInject(name string, value interface{}) *Web`
   向所有模板页面中注入单个数据。
 
 - `func (web *Web) PageInjectMap(dataMap map[string]interface{}) *Web`
   向所有模板页面中注入多个数据。
   
 - `func (web *Web) PageInjectSession(name, sessionName string) *Web`
   向所有模板页面中注入指定名称的 session。
   name 为模板中使用的变量名。
 
 - `func (web *Web) PageInjectSessionMap(sessionMap map[string]string) *Web`
   向所有模板页面中注入多个 session。
 
 - `func (web *Web) Debug(debug bool) *Web`
   设置是否启动调试，启动调试后，终端将会输出额外信息。
 
 - `func (web *Web) Handler() http.Handler`
   返回 Web 服务定义对应的 http.Handler。

### 配置定义

配置定义对象用于快速处理 Web 服务的配置。

结构：

	type WebConfig config.Config

接口：

	func NewConfig(cfg *config.Config) *WebConfig
	func (wc *WebConfig) GetDebug() bool 
	func (wc *WebConfig) GetBaseUrl() string
	func (wc *WebConfig) GetPageInjectData() map[string]interface{}
	func (wc *WebConfig) GetPageInjectSessions() map[string]string
	func (wc *WebConfig) GetPages() []*Page
	func (wc *WebConfig) GetAPIs() []*API
	func (wc *WebConfig) GetStatics() []*Static
