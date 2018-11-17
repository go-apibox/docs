API连线检测
============

该中间件实现了一个简易的 API 接口，用于检测 API 系统是否可连接。

接口名为：`Ping`

请求该接口将返回如下数据：

	{
		"ACTION": "Ping",
		"CODE": "ok",
		"DATA": null
	}

如果启用 websocket 协议，还可以使用 websocket 协议请求该接口。
如果希望自行处理 websocket 连接，可以通过 `SetWsHandler()` 函数传入处理函数。

### 相关配置

- apiping：
    API连线检测中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。
	  
	- websocket_enabled:
	  是否启用websocket协议支持，不配置则默认为：`false`。

      
配置示例：

	apiping:
	  disabled: true

###接口参考：

- `func (p *Ping) SetWsHandler(handler apiping.WsHandler)`
  设置 websocket 处理函数。
  `apiping.WsHander` 类型的声明为：
    `type WsHandler func(conn *websocket.Conn) error`

###使用示例：

	package main
	
	import (
		"fmt"
		"os"
	
		"github.com/go-apibox/api"
		"github.com/go-apibox/apiping"
	)
	
	func main() {
		app, err := newApp()
		if err != nil {
			fmt.Println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apiping.NewPing(app))
	
		app.Run([]*api.Route{})
	}

