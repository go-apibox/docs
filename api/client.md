客户端
======

使用API客户端必须引入 `git.quyun.com/apibox/apiclient`。

## 接口参考

### Client

 - `func NewClient(gwURL string) *Client`
    创建客户端，结构如下：

		type Client struct {
			// 网关设置，如果GWADDR为空，则通过DNS查询服务器IP
			GWURL     string
			GWADDR    string
			gwURLInfo *url.URL
		
			// 签名设置，SignKey为空表示不进行签名
			AppId   string
			SignKey string
		
			// Nonce设置，长度为0表示不添加Nonce参数
			NonceEnabled bool
			NonceLength  int
		
			// 默认请求参数设置
			DefaultParams map[string]string
			
			// 要强制覆盖的请求参数设置
			OverrideParams map[string]string
		}

  默认创建值如下：
   - `GWADDR`：空字符串
   - `AppId`：空字符串
   - `SignKey`：空字符串
   - `NonceEnabled`：false
   - `NonceLength`：16
   - `DefaultParams`：空

  创建后，可通过修改属性值 对客户端进行设置。

 - `func (client *Client) SetDefaultParam(paramName, paramValue string) *Client`
   设置默认请求参数值，当请求参数不存在时，使用该默认值。

 - `func (client *Client) SetOverrideParam(paramName, paramValue string) *Client`
   设置要强制覆盖的请求参数值，无法请求参数存不存在，都使用该值覆盖。

 - `func (client *Client) Get(action string, params url.Values, header http.Header) (*Response, error)`
    以 HTTP GET 方法请求 API。

 - `func (client *Client) Post(action string, params url.Values, header http.Header) (*Response, error)`
    以 HTTP POST 方法请求 API。

 - `func (client *Client) WsConn(action string, params url.Values, header http.Header) (*websocket.Conn, *http.Response, error)`
    以 Websocket 协议连接 API。
 
 - `func (client *Client) Websocket(action string, params url.Values, header http.Header, w http.ResponseWriter, r *http.Request) (*Response, error)`
    以 Websocket 方法请求 API，并将当前请求升级为 Websocket 作为客户端。

### Response

 - `func (resp *Response) String() string`
   返回响应内容字符串。
 
 - `func (resp *Response) Json() (*simplejson.Json, error)`
    以 `*simplejson.Json` 类型返回响应内容。

 - `func (resp *Response) Result() (*api.Result, error)`
   以 `*api.Result` 类型返回响应内容。

 - `func (resp *Response) RequestId() string`
   返回 HTTP 响应头部中包含的 `X-Request-Id` 的值。

## 使用示例

	package main
	
	import (
		"fmt"
		"net/url"
	
		"git.quyun.com/apibox/apiclient"
	)
	
	func main() {
	
		c := apiclient.NewClient("http://192.168.122.120:8080/")
	
		// 启用签名
		c.SignKey = "123456"
	
		// 启用重攻击防护
		c.NonceEnabled = true

		// 设置默认参数
		c.SetParam("api_debug", "1")
		
		// Action参数
		params := make(url.Values)
		params.Set("ChannelId", "1866514129")
	
		resp, err := c.Post("Channel.List", params, nil)
		if err != nil {
			fmt.Println(err)
			return
		}
	
		// 请求ID
		fmt.Println("Request Id:", resp.RequestId())
	
		// 返回响应原文
		fmt.Println("\nas String\n-------------------")
		fmt.Println(resp.String())
	
		// 返回*api.Result
		fmt.Println("\nas *api.Result\n-------------------")
		fmt.Println(resp.Result())
	
		// 返回*simplejson.Json
		fmt.Println("\nas *simplejson.Json\n-------------------")
		fmt.Println(resp.Result())
	}

打印结果如下：

	# go run main.go 
	Request Id: 2d4aba73-9674-11e4-86c6-a1d24521e878
	
	as String
	-------------------
	{
	    "ACTION": "Channel.List",
	    "CODE": "ok",
	    "DATA": {
	        "Channel[]": [
	            {
	                "ChannelId": 1866514129,
	                "ChannelKey": "gVRXTRwQPRy3lM18",
	                "ChannelName": "appnode4",
	                "CreateTime": 1420458149
	            }
	        ]
	    }
	}
	
	as *api.Result
	-------------------
	&{Channel.List ok  map[Channel[]:[map[ChannelKey:gVRXTRwQPRy3lM18 ChannelName:appnode4 CreateTime:1.420458149e+09 ChannelId:1.866514129e+09]]]} <nil>
	
	as *simplejson.Json
	-------------------
	&{Channel.List ok  map[Channel[]:[map[ChannelId:1.866514129e+09 ChannelKey:gVRXTRwQPRy3lM18 ChannelName:appnode4 CreateTime:1.420458149e+09]]]} <nil>