IP
======

IP 过滤器用于对 IP 地址进行过滤。

#### 新建过滤器：

	ipFilter := filter.IP()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："127.0.0.1"
 - `net.IP`：如：net.ParseIP("127.0.0.1")
 - `*net.IP`：如：&net.ParseIP("127.0.0.1")

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *IPFilter) Allow(vals ...string) *IPFilter`
   允许参数值为指定列表中的字符串。

以下过滤器设置会在验证后对参数值进行处理：

 - `func (f *IPFilter) ToString() *IPFilter`
   验证完后将参数值转换为字符串返回。

#### 验证规则：

 - `func (f *IPFilter) IsIPv4() *IPFilter`
   IP 地址必须为IPv4。
 
 - `func (f *IPFilter) IsIPv6() *IPFilter`
   IP 地址必须为IPv6。

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *IPFilter) AddValidator(validator IPValidator) *IPFilter

验证函数类型如下：

	type IPValidator func(paramName string, paramValue *net.IP) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error