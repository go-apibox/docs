CIDR
======

CIDR 过滤器用于对 CIDR 地址进行过滤。

#### 新建过滤器：

	cidrFilter := filter.CIDR()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："192.168.1.1/24"

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *CIDRFilter) Allow(vals ...string) *CIDRFilter`
   允许参数值为指定列表中的字符串。

以下过滤器设置会在验证后对参数值进行处理：

 - `func (f *CIDRFilter) ToString() *CIDRFilter`
   验证完后将参数值转换为字符串返回。

#### 验证规则：

 - `func (f *CIDRFilter) IsIPv4() *CIDRFilter`
   CIDR 地址必须为IPv4。
 
 - `func (f *CIDRFilter) IsIPv6() *CIDRFilter`
   CIDR 地址必须为IPv6。

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *CIDRFilter) AddValidator(validator CIDRValidator) *CIDRFilter

验证函数类型如下：

	type CIDRValidator func(paramName string, paramValue *CIDRAddr) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error