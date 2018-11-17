路由
======

APIBox 根据请求参数中的 `api_action` 来进行路由，路由规则定义了 `api_action` 与对应 Action 处理函数的对应关系。

路由规则结构如下：

    type Route struct {
    	ActionCode string
    	ActionFunc ActionFunc
    	Hooks      map[string]ActionFunc
    }

 - `ActionCode`：
    API 接口名称，即请求参数中 `api_action` 的值
   
 - `ActionFunc`：
    API Action 处理函数，格式为：`func(c *Context) (data interface{})`
   
 - `Hooks`：
	可对当前 Action 配置以下钩子 Action：
	
	 - `BeforeAction`：在所有 Action 执行前执行，如返回非 nil，则不再执行后续 Action。
	 - `AfterAction`：在所有 Action 执行后执行，如主 Action 返回值非 nil，钩子 Action 的返回值将被忽略。上一步 Action 的执行结果会保存在名为 `output` 的 context 变量中，在钩子 Action 中可以通过 `c.Get("output")` 来获取。
	
	可通过以下接口添加钩子 Action：
	
	 - `func (r *Route) Hook(tag string, actionFunc ActionFunc) *Route`
	   参数 tag 可取值为：`BeforeAction`、`AfterAction`    


一般我们会在应用项目目录下新建 `routes.go` 来存放该应用的所有路由规则，如：

	// 路由配置
	
	package main
	
	import (
		"github.com/go-apibox/api"
	)
	
	var apiRoutes = []*api.Route{
		api.NewRoute("Test.Ok", TestOkAction),
		api.NewRoute("Test.Error", TestErrorAction),
	}

路由配置在 `app.Run()` 时作为参数传入应用，如：

    package main
    
    import (
    	"os"
    
    	"github.com/go-apibox/api"
    )
    
    func main() {
    	app, err := api.NewApp()
    	if err != nil {
    		println(err.Error())
    		os.Exit(-1)
    	}
    	app.Run(apiRoutes)
    }

