API总线
============

### 实现原理

可将多个 API 系统接入 API 总线，达到统一 API 入口的目的。

要点：

 - API 总线通过请求参数 `api_module` 来判断要将请求代理到哪个后端的 API 系统中。
 - 当后端 API 进程未启动时，API 总线会尝试启动 API 进程后再将请求代理过去。
 - API 总线要与 API 代理中间件一同使用才会生效，否则将返回 500 内部错误。
 - 后端的 API 系统中，需要使用 API 总线模块适配器（apibox/apiinit）才能正常使用。
 - 如果后端 API 系统闲置（没有发出或收到请求超过5分钟），相应的 API 进程将被关闭。

### 相关配置

- apibus：
    API总线中间件配置组

    - disabled:
      是否禁用该中间件，不配置则默认为：`false`。

    - exe_path：
      后端 API 系统的可执行文件路径模板，默认为：`./modules/{module}/{module}`。
      
    - sock_path：
      后端 API 系统运行时要监听的 unix domain socket 文件路径模板，默认为：`./run/{module}.sock`。
      
    - db_path：
      后端 API 系统的数据库文件路径模板，默认为：`./db/{module}.db`。

    - stat_gateway:
      统计服务器网关地址，默认不设置表示不统计。
      统计数据将会定期POST到统计网关，POST参数：
      - 参数键：`Data`
      - 参数值：`192.168.1.120,1446106347,sysinfo|1|5462,nginx|19|122`
        第一个字段是出口网卡IP
        第二个字段是开始统计的时间戳
        后续字段的格式为：模块|启动次数|调用次数

    - stat_interval:
      统计数据上报时间间隔，单位为秒，不配置则默认为3600。
      每次数据上报完成后，计数将被清零。

上述配置中 `{module}` 将被自动替换为 `api_module` 请求参数值。
考虑安全因素，该中间件要求 `api_module` 参数值只能由数字、字母、下划线、横杠组成，不满足条件的参数值将返回 404 错误。
      
配置示例：

	apibus:
	  exe_path: "../apps/{module}"
	  sock_path: "../run/{module}.sock"
	  db_path: "./db/{module}.db"

### 相关错误

| 错误码                     | 错误消息                     | 错误描述            |
| ------------------------- | --------------------------- | ------------------ |
| WrongModulePath           | Wrong module path!          | 模块路径错误！       |
| ModuleStartFailed         | Module start failed!        | 模块启动失败！       |


###使用示例：

	package main
	
	import (
		"fmt"
		"os"
	
		"github.com/go-apibox/api"
		"github.com/go-apibox/apibus"
		"github.com/go-apibox/apiproxy"
	)
	
	func main() {
		app, err := newApp()
		if err != nil {
			fmt.Println(err.Error())
			os.Exit(1)
		}
	
		app.Use(apibus.NewBus(app))
		app.Use(apiproxy.NewProxy(app))
	
		app.Run([]*api.Route{})
	}

