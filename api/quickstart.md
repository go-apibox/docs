快速开始
==========

本文将使用 APIBox 快速创建一个简单的 API 接口。

##创建应用

创建名称为 myapi 的应用：

    # cd $GOPATH/src
    # mkdir myapi
    # cd myapi/

新建应用入口文件 app.go：

    package main
    
    import (
    	"fmt"
    	"os"
    
    	"git.quyun.com/apibox/api"
    )
    
    func main() {
		app, err := api.NewApp()
		if err != nil {
			fmt.Println(err.Error())
			os.Exit(1)
		}
    	app.Run(apiRoutes)
    }

`api.NewApp()` 将会创建一个应用，默认将读取 config/app.yaml 作为应用的配置。

## 应用配置

应用配置文件使用 YAML 格式来编写，新建示例配置文件 `config/app.yaml`：

	# config of application
	app:
	  name: myapi
	  http_addr: :8080

关于应用配置的详细信息，以及支持的配置项请参考：

 - [应用](app.md)
 

##路由配置

app.go 的 `app.Run` 函数的参数 apiRoutes 为 API 路由设置，在 routes.go 中定义：

    // 路由配置

	package main
	
	import (
		"git.quyun.com/apibox/api"
	)
	
	var apiRoutes = []*api.Route{
		api.NewRoute("Test.Ok", TestOkAction),
		api.NewRoute("Test.Error", TestErrorAction),
	}

Route 结构的三个成员分别是：{API接口名称，API接口要调用的函数，API请求上下文初始化函数}。

关于路由的详细说明，具体请参考：

 - [路由](router.md)

路由配置好后，只需要实现与 API 相关的 Action 函数即可。

##API Action实现

上例中的 TestOkAction 和 TestErrorAction 位于文件 actons.go 中：

	// Action测试
	
	package main
	
	import (
		"git.quyun.com/apibox/api"
	)
	
	func TestOkAction(c *api.Context) interface{} {
		return "ok"
	}
	
	func TestErrorAction(c *api.Context) interface{} {
		return c.Error.New(api.ErrorInvalidParam, "Password", "TooShort")
	}


##应用目录结构

该应用最终的目录结构如下：

    # tree $GOPATH/src/myapi/    
    /root/godev/src/myapi/
    ├── actions.go
    ├── app.go
    ├── config
    │   └── app.yaml
    └── routes.go

## 编译运行和测试

编译运行：

    # go build && ./myapi 
    [myapi] listening on :8080

测试：

    # curl "http://127.0.0.1:8080/?api_action=Test.Ok&api_debug=1"   
    {
        "ACTION": "Test.Ok",
        "CODE": "ok",
        "DATA": "ok"
    }
    # curl "http://127.0.0.1:8080/?api_action=Test.Error&api_debug=1"
	{
	    "ACTION": "Test.Error",
	    "CODE": "InvalidParam:Password:TooShort",
	    "MESSAGE": "Invalid param Password: too short!"
	}

