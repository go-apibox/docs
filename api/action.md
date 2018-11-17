Action
======

Action 在 APIBox 中用于表示 API 接口对应的操作函数，每个 API 接口均由一个对应的 Action 函数实现。

Action 函数类型 `ActionFunc` 定义为：

	type ActionFunc func(c *Context) (data interface{})


### 函数参数

其中参数 `c` 为请求上下文，其中包含了应用、输入、输出和错误处理相关模块成员，详细请参考：

 - [请求上下文](context.md)

我们可以通过 `c` 中的 `Error` 模块来返回一个 API 错误，如：

	func TestAction(c *api.Context) interface{} {
		return c.Error.New(api.ErrorObjectNotExist)
	}


### 返回值

`ActionFunc` 的返回值是 `interface{}` 类型，即任意类型。

 * 返回值类型为非 `api.Error` 

   表示返回成功，APIBox 接收返回值后会把它作为 API 返回数据输出到 DATA 段中，如：
   
		{
		    "ACTION": "Action.Name",
		    "CODE": "ok",
		    "DATA": ... // 此处为返回值
		}

 * 返回值类型为 `api.Error` 
 
   表示返回错误，APIBox 接收到该类型返回值后会输出相应的错误信息，如：

		{
		    "ACTION": "Action.Name",
		    "CODE": ..., // 错误代码
		    "Message": ... // 错误消息
		}

### 内置 Action

APIBox 内置了一些预定义的 Action，方便用于查看应用信息：

	// 返回 Cookie 密钥对
	func APIBoxSessionGetKeyAction(c *Context) interface{}