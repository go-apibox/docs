参数处理
========

需要使用 APIBox 的参数处理过滤功能，需要导入 filter 类库：

	import "git.quyun.com/apibox/filter"

关于 filter 类库的相关文档，请参考这里：

 - [filter类库](../filter/quickstart.md)

APIBox 实现了一个 `Params` 类型，用于对 HTTP 请求参数进行验证，验证规则使用的是由 filter 类库提供的验证功能。

## 方法参考

`Params` 封装了以下方法：

 - `func (p *Params) Add(paramName string, filters ...filter.Filter)`
   添加请求参数，可以支持多个过滤器，前一个过滤器的返回结果将作为后一个过滤器的输入值。

 - `func (p *Params) AddPagination() *Params`
   快速添加分页相关的验证规则，相关参数包括：
    - `PageNumber`：默认值为 `1`
    - `PageSize`：默认值为 10，取值范围为：[1,100]
   
 - `func (p *Params) AddOrderBy(defaultOrderBy string, defaultOrder string, allowOrderBys []string, allowMultiOrder bool) *Params`
   设置排序相关的验证规则，相关参数包括：
    - `Order`：默认值为 `asc`，合法值为：`asc`、`desc`
    - `OrderBy`：默认值为 defaultOrderBy，合法值为：allowOrderBys
  
   如果参数 allowMultiOrder 为 true，则允许多列排序。

 - `func (p *Params) Del(paramName string) *Params`
   删除请求参数。

 - `func (p *Params) Parse() *Error`
   根据设置的过滤器规则，分析验证请求参数。如分析验证成功，则返回 nil，否则返回 `*Error`。

 - `func (p *Params) Set(paramName string, paramValue interface{}) *Params`
   设置指定请求参数的分析结果值。
 
 - `func (p *Params) Has(paramName string) bool`
   获取指定请求参数的分析结果值中是否存在指定参数。

 - `func (p *Params) Get(paramName string) interface{}`
   获取指定请求参数的分析结果值。

 - `func (p *Params) GetAll() map[string]interface{}`
   获取所有请求参数的分析结果值。
 
 - `func (p *Params) GetString(paramName string) string`
   以 `string` 类型获取指定请求参数的分析结果值。

 - `func (p *Params) GetInt(paramName string) int`
   以 `int` 类型获取指定请求参数的分析结果值。如果值不是 `int` 类型，则返回 0。
 
 - `func (p *Params) GetInt32(paramName string) int32`
   以 `int32` 类型获取指定请求参数的分析结果值。如果值不是 `int32` 类型，则返回 0。

 - `func (p *Params) GetInt64(paramName string) int64`
   以 `int64` 类型获取指定请求参数的分析结果值。如果值不是 `int64` 类型，则返回 0。

 - `func (p *Params) GetUint(paramName string) uint`
   以 `uint` 类型获取指定请求参数的分析结果值。如果值不是 `uint` 类型，则返回 0。

 - `func (p *Params) GetUint32(paramName string) uint32`
   以 `uint32` 类型获取指定请求参数的分析结果值。如果值不是 `uint32` 类型，则返回 0。

 - `func (p *Params) GetUint64(paramName string) uint64`
   以 `uint64` 类型获取指定请求参数的分析结果值。如果值不是 `uint64` 类型，则返回 0。

 - `func (p *Params) GetTime(paramName string) *time.Time`
   以 `*time.Time` 类型获取指定请求参数的分析结果值。如果值不是 `*time.Time`或 `time.Time` 类型，则返回 nil。

 - `func (p *Params) GetTimestamp(paramName string) uin32`
   以 `uint32` 类型获取指定请求参数的分析结果值。如果值不是 `uin32` 类型，则返回 0。

 - `func (p *Params) GetIntRange(paramName string) *types.IntRange`
   以 `*types.IntRange` 类型获取指定请求参数的分析结果值。如果值不是 `*types.IntRange` 或 `types.IntRange` 类型，则返回 nil。
   
 - `func (p *Params) GetInt32Range(paramName string) *types.Int32Range`
   以 `*types.Int32Range` 类型获取指定请求参数的分析结果值。如果值不是 `*types.Int32Range` 或 `types.Int32Range` 类型，则返回 nil。

 - `func (p *Params) GetInt64Range(paramName string) *types.Int64Range`
   以 `*types.Int64Range` 类型获取指定请求参数的分析结果值。如果值不是 `*types.Int64Range` 或 `types.Int64Range` 类型，则返回 nil。

 - `func (p *Params) GetUintRange(paramName string) *types.UintRange`
   以 `*types.UintRange` 类型获取指定请求参数的分析结果值。如果值不是 `*types.UintRange` 或 `types.UintRange` 类型，则返回 nil。
  
 - `func (p *Params) GetUint32Range(paramName string) *types.Uint32Range`
   以 `*types.Uint32Range` 类型获取指定请求参数的分析结果值。如果值不是 `*types.Uint32Range` 或 `types.Uint32Range` 类型，则返回 nil。
   
 - `func (p *Params) GetUint64Range(paramName string) *types.Uint64Range`
   以 `*types.Uint64Range` 类型获取指定请求参数的分析结果值。如果值不是 `*types.Uint64Range` 或 `types.Uint64Range` 类型，则返回 nil。
 
 - `func (p *Params) GetTimeRange(paramName string) *types.TimeRange`
   以 `*types.TimeRange` 类型获取指定请求参数的分析结果值。如果值不是 `*types.TimeRange` 或 `types.TimeRange` 类型，则返回 nil。
 
 - `func (p *Params) GetTimestampRange(paramName string) *types.TimestampRange`
   以 `*types.TimestampRange` 类型获取指定请求参数的分析结果值。如果值不是 `*types.TimestampRange` 或 `types.TimestampRange` 类型，则返回 nil。

 - `func (p *Params) GetStringArray(paramName string) []string`
   以 `[]string` 类型获取指定请求参数的分析结果值。如果值不是 `[]string` 类型，则返回 []string{}。

 - `func (p *Params) GetIntArray(paramName string) []int`
   以 `[]int` 类型获取指定请求参数的分析结果值。如果值不是 `[]int` 类型，则返回 []int{}。

 - `func (p *Params) GetInt32Array(paramName string) []int32`
   以 `[]int32` 类型获取指定请求参数的分析结果值。如果值不是 `[]int32` 类型，则返回 []int32{}。
 
 - `func (p *Params) GetInt64Array(paramName string) []int64`
   以 `[]int64` 类型获取指定请求参数的分析结果值。如果值不是 `[]int64` 类型，则返回 []int64{}。

 - `func (p *Params) GetUintArray(paramName string) []uint`
   以 `[]uint` 类型获取指定请求参数的分析结果值。如果值不是 `[]uint` 类型，则返回 []uint{}。

 - `func (p *Params) GetUintArray(paramName string) []uint32`
   以 `[]uint32` 类型获取指定请求参数的分析结果值。如果值不是 `[]uint32` 类型，则返回 []uint32{}。

 - `func (p *Params) GetUint64Array(paramName string) []uint64`
   以 `[]uint64` 类型获取指定请求参数的分析结果值。如果值不是 `[]uint64` 类型，则返回 []uint64{}。

 - `func (p *Params) GetTimeArray(paramName string) []*time.Time`
   以 `[]*time.Time` 类型获取指定请求参数的分析结果值。如果值不是 `[]*time.Time` 类型，则返回 []*time.Time{}。


## 使用示例一

	package main
	
	import (
		"git.quyun.com/apibox/api"
		"github.com/go-apibox/filter"
	)
	
	func TestParamAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("Email", filter.Required(), filter.Email().Domain("gmail.com"))
		if err := params.Parse(); err != nil {
			return err
		}
	
		email := params.GetString("Email")
		return email
	}

示例解说：
1. 通过调用 Context 中封装的 `NewParams()` 返回一个 `Params` 对象
2. `params.Add()` 在请求参数 Email 上添加了两个过滤器：
    2.1. Email 参数是必传的
    2.2. Email 参数必须是合法的 Email 格式，且必须以 gmail.com 为邮箱域名
3. `params.Parse()` 按照定义的过滤器规则对请求参数进行分析验证，如果验证错误则返回错误
4. `params.GetString()` 以 `string` 类型获取 Email 参数值，并最终输出

测试：

	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1"
	{
	    "ACTION": "Test.Param",
	    "CODE": "MissingParam:Email",
	    "MESSAGE": "Missing param Email!"
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&Email=hilyjiang@qq.com" 
	{
	    "ACTION": "Test.Param",
	    "CODE": "InvalidParam:Email:WrongFormat",
	    "MESSAGE": "Invalid param Email: wrong format!"
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&Email=hilyjiang@gmail.com"
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": "hilyjiang@gmail.com"
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&Email=HILYJIANG@GMAIL.COM"
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": "hilyjiang@gmail.com"
	}

## 使用示例二

	package main

	import (
		"github.com/go-apibox/api"
		"github.com/go-apibox/filter"
	)
	
	func TestParamAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("File", filter.StringSet().MaxCount(3))
		if err := params.Parse(); err != nil {
			return err
		}
	
		return params.GetStringArray("File")
	}

示例解说：
1. 通过调用 Context 中封装的 `NewParams()` 返回一个 `Params` 对象
2. `params.Add()` 在请求参数 File 上添加了一个过滤器：
    File 参数是一个字符串集合，并且个数不能超过 3 个。
3. `params.Parse()` 按照定义的过滤器规则对请求参数进行分析验证，如果验证错误则返回错误
4. `params.GetStringArray()` 以 `[]string` 类型获取 File 参数值，并最终输出

测试：

	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1"
	{
	    "ACTION": "Test.Param",
	    "CODE": "InvalidParam:File:NotStringSet",
	    "MESSAGE": "Invalid param File: not string set!"
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&File=file1.txt&File=file2.txt"
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": [
	        "file1.txt",
	        "file2.txt"
	    ]
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&File=file1.txt,file2.txt" 
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": [
	        "file1.txt",
	        "file2.txt"
	    ]
	}
	# curl "http://127.0.0.1:8080/?api_action=Test.Param&api_debug=1&File=f1.txt,f2.txt,f3.txt,f4.txt"
	{
	    "ACTION": "Test.Param",
	    "CODE": "InvalidParam:File:TooMany",
	    "MESSAGE": "Invalid param File: too many!"
	}

## 使用示例三

	package main
	
	import (
		"github.com/go-apibox/api"
		"github.com/go-apibox/filter"
	)
	
	type User struct {
		Username string
	}
	
	func TestParamAction(c *api.Context) interface{} {
		user := new(User)
	
		params := c.NewParams()
		params.Add("User", filter.Json().Output(user))
		if err := params.Parse(); err != nil {
			return err
		}
	
		return user
	}

示例解说：
1. 通过调用 Context 中封装的 `NewParams()` 返回一个 `Params` 对象
2. `params.Add()` 在请求参数 File 上添加了一个过滤器：
    User 参数是一个 json 字符串，解析成功后输出到变量 user 中。
3. `params.Parse()` 按照定义的过滤器规则对请求参数进行分析验证，如果验证错误则返回错误
4. 最终直接返回 user 变量

测试：

	# curl "http://192.168.122.120:8080/?api_action=Test.Param&api_debug=1&User=%7B%22Username%22:%22hilyjiang%22%7D"
	{
	    "ACTION": "Test.Param",
	    "CODE": "ok",
	    "DATA": {
	        "Username": "hilyjiang"
	    }
	}

上述示例代码也可以改为直接使用过滤器的执行结果，输出结果是一样的：

	package main
	
	import (
		"github.com/go-apibox/api"
		"github.com/go-apibox/filter"
	)
	
	func TestParamAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("User", filter.Json())
		if err := params.Parse(); err != nil {
			return err
		}
	
		return params.Get("User")
	}
