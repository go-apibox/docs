API风格
=======

基于 APIBox 创建的 API 应用，会有一致的风格。

基本的请求与响应格式如下：

### 请求格式

    http://api.example.com/?api_action=Action.Name&api_debug=1&Param1=...

说明：

 - 支持通过 HTTP GET 或 HTTP POST 来传递API参数
 - 每个以 `api_` 开始的参数，为全局参数
 - 非全局参数（业务参数）的参数名称均以大写字母开头

### 响应格式

成功结果示例（json）：

    {
        "ACTION": "Action.Name",
        "CODE": "ok",
        "DATA": {
            …
        }
    }

失败结果示例（json）：

    {
        "ACTION": "Action.Name",
        "CODE": "ActionNotExist",
        "MESSAGE": "Action does not exsit!",
        "DATA": {
            …
        }
    }

响应结果中各字段的含义：

 - `ACTION`：当前调用的 API 接口名称
 - `CODE`：调用返回代码，`ok` 表示调用成功，否则表示具体的错误码
 - `DATA`：调用返回的数据（在调用失败时该项为可选）
 - `MESSAGE`：调用失败时返回的错误消息（仅在调用失败时返回）

对于调用失败时返回的错误码和错误消息，请参考：

 - [错误码格式](api/errorcode.md)
 - [错误消息自动生成](api/errormsg.md)

