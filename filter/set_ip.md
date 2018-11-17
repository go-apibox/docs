IPSet
======

IPSet 过滤器用于对 IP 地址集合进行过滤。

#### 新建过滤器：

	ipsFilter := filter.IPSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："127.0.0.1,218.85.157.99", "192.168.1.1"
 - `[]string`：如：[]string{"127.0.0.1", "218.85.157.99"}, []string{"192.168.1.1"}
 - `[]net.IP`：如：[]net.IP{net.ParseIP("127.0.0.1"), net.ParseIP("218.85.157.99")}
 - `[]*net.IP`：如：[]*net.IP{&net.ParseIP("127.0.0.1"), &net.ParseIP("218.85.157.99")}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *IPSetFilter) Allow(vals ...string) *IPSetFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *IPSetFilter) Delimiter(delimiter string) *IPSetFilter`
   指定集合分隔符，默认为：`,`。

 - `func (f *IPSetFilter) MinCount(count int) *IPSetFilter`
   集合允许的最少元素数量。

 - `func (f *IPSetFilter) MaxCount(count int) *IPSetFilter`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *IPSetFilter) ItemIsIPv4() *IPSetFilter`
   集合中的 IP 地址必须为IPv4。
 
 - `func (f *IPSetFilter) ItemIsIPv6() *IPSetFilter`
  集合中的  IP 地址必须为IPv6。

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *IPSetFilter) AddValidator(validator IPSetValidator) *IPFilter

验证函数类型如下：

	type IPSetValidator func(paramName string, paramValue []*net.IP) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error