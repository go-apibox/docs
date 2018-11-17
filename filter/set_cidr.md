CIDRSet
======

CIDRSet 过滤器用于对 CIDR 值集合进行过滤。

#### 新建过滤器：

	ipsFilter := filter.CIDRSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："192.168.1.1/24,192.168.2.1/24", "192.168.1.1/24"
 - `[]string`：如：[]string{"192.168.1.1/24", "192.168.2.1/24"}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *CIDRSetFilter) AllowEmpty() *CIDRSetFilter`
   允许参数为空字符串。

 - `func (f *CIDRSetFilter) Delimiter(delimiter string) *CIDRSetFilter`
   指定集合分隔符，默认为：`,`。

 - `func (f *CIDRSetFilter) MinCount(count int) *CIDRSetFilter`
   集合允许的最少元素数量。

 - `func (f *CIDRSetFilter) MaxCount(count int) *CIDRSetFilter`
   集合允许的最多元素数量。

以下过滤器设置会在验证后对参数值进行处理：

 - `func (f *CIDRSetFilter) ToString() *CIDRSetFilter`
   验证完后将参数值转换为字符串返回。

#### 验证规则：

 - `func (f *CIDRSetFilter) ItemIsIPv4() *CIDRSetFilter`
   集合中的 CIDR 值必须为IPv4。
 
 - `func (f *CIDRSetFilter) ItemIsIPv6() *CIDRSetFilter`
  集合中的  CIDR 值必须为IPv6。

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *CIDRSetFilter) AddValidator(validator CIDRSetValidator) *IPFilter

验证函数类型如下：

	type CIDRSetValidator func(paramName string, paramValue []*net.IP) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error