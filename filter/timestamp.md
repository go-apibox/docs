Timestamp
==========

Timestamp 过滤器用于对Unix时间戳进行过滤。

#### 新建过滤器：

	tsFilter := filter.Timestamp()
	
该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - string：如："1419314831"
 - uint：如：1419314831

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimestampFilter) Allow(vals ...string) *TimestampFilter`
   允许参数值为指定列表中的字符串。

#### 验证规则：

 - `func (f *TimestampFilter) StartFrom(val uint) *TimestampFilter`
   时间戳不能小于 val（&gt;=val）
 
 - `func (f *TimestampFilter) EndTo(val uint) *TimestampFilter`
   时间戳不能大于 val（&lt;=val）

 - `func (f *TimestampFilter) Before(val uint) *TimestampFilter`
   时间戳必须小于 val（&lt;val）

 - `func (f *TimestampFilter) After(val  uint) *TimestampFilter`
   时间戳必须大于 val（&gt;val）

 - `func (f *TimestampFilter) Equal(val  uint) *TimestampFilter`
   时间戳必须等于 val（=val）

 - `func (f *TimestampFilter) Between(start, end uint) *TimestampFilter`
   时间戳必须在 start 和 end 之间 （&gt;=start且&lt;=end）

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimestampFilter) AddValidator(validator TimestampValidator) *TimestampFilter

验证函数类型如下：

	type TimestampValidator func(paramName string, paramValue uint) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error