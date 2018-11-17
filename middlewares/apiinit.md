API初始化
============

### 实现原理

该中间件会在 API 系统进程后，提供一个初始化 API 系统的接口，供外部调用。

一般使用场景：

 - 在初始化操作前，除了该接口，其它接口均无法调用。
 - 在初始化操作完成后，该接口将无法再被调用。

一般情况下要将该接口置于其它中间件之前。

初始化 API 接口名为：`APIBox.Init`
请求参数如：`apisign.sign_key=123456&apisign.enabled=true`

目前支持对以下中间件的配置进行初始化：

 - apisign.sign_key
 - apisign.enabled
 - apiproxy.{backend}.gwurl
 - apiproxy.{backend}.gwaddr
 - apiproxy.{backend}.appid
 - apiproxy.{backend}.sign_key
 - apiproxy.{backend}.nonce_enabled
 - apiproxy.{backend}.nonce_length
 - apiproxy.{backend}.default_params.{param_name}
 - apiproxy.{backend}.override_params.{param_name}

 
以上列表中 `{backend}` 为后端代理别名，`{param_name}` 为具体的参数名。


### 相关配置

- apiinit：
    API初始化中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。

      
配置示例：

	apiinit：
	  disabled: true


###使用示例：

	package main
	
	import (
		"fmt"
		"os"
	
		"git.quyun.com/apibox/api"
		"git.quyun.com/apibox/apiinit"
		"git.quyun.com/apibox/apisign"
	)
	
	func main() {
		app, err := newApp()
		if err != nil {
			fmt.Println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apiinit.NewInit(app))
		app.Use(apisign.NewSign(app))
	
		app.Run([]*api.Route{})
	}

