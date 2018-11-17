输出错误
========

在 Action 中输出错误，可调用 `c.Error.New(errorType ErrorType, fields ...string)` 来实现，如：

    // Action测试
    
    package main
    
    import (
    	"github.com/go-apibox/api"
    )
    
    func TestErrorAction(c *api.Context) interface{} {
    	return c.Error.New(api.ErrorInvalidParam, "Password", "TooShort")
    }

APIBox 在执行完 Action 函数后会将返回值中的错误信息自动格式化并输出，上述示例输出的错误信息如下：

	# curl "http://127.0.0.1:8080/?api_action=Test.Error&api_debug=1"
	{
	    "ACTION": "Test.Error",
	    "CODE": "InvalidParam:Password:TooShort",
	    "MESSAGE": "Invalid param Password: too short!"
	}

返回值中的 `CODE` 和 `MESSAGE` 都是根据 `c.Error.New()` 参数自动生成的。

### 自定义错误消息 

可通过在 Error 对象上调用以下方法设置自定义错误消息：

	func (e *Error) SetMessage(message string) *Error

### 返回错误数据

可通过在 Error 对象上调用以下方法将错误数据附加到返回结果中：

	func (e *Error) SetData(data interface{}) *Error

---------

更多关于 API 请求/响应格式的信息，请参考：

 - [API风格](../style.md)

### 本节目录

1. [错误码格式](errorcode.md)
2. [错误消息自动生成](errormsg.md)