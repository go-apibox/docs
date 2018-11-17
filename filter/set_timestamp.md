TimestampSet
======================================

TimestampSet 过滤器用于对时间戳集合参数进行过滤。

#### 新建过滤器：

	tsFilter := filter.TimestampSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："0,1419314831,1419407205", "1419314831"
 - `[]string`：如：[]string{"0", "1419314831", "1419407205"}
 - `[]uint`：如：[]uint{0, 1419314831, 1419407205}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimestampSetFilter) Allow(vals ...string) *TimestampSetFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *TimestampSetFilter) Delimiter(delimiter string) *TimestampSetFilter`
   指定集合分隔符，默认为：`,`。

 - `func (f *TimestampSetFilter) MinCount(count int) *TimestampSetFilter`
   集合允许的最少元素数量。

 - `func (f *TimestampSetFilter) MaxCount(count int) *TimestampSetFilter`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *TimestampSetFilter) ItemStartFrom(val uint) *TimestampSetFilter`
   集合中元素值不能小于 val（&gt;=val）

 - `func (f *TimestampSetFilter) ItemEndTo(val uint) *TimestampSetFilter`
   集合中元素值不能大于 val（&lt;=val）

 - `func (f *TimestampSetFilter) ItemAfter(val uint) *TimestampSetFilter`
   集合中元素值必须大于 val（&lt;val）

 - `func (f *TimestampSetFilter) ItemBefore(val uint) *TimestampSetFilter`
   集合中元素值必须小于 val（&gt;val）

 - `func (f *TimestampSetFilter) ItemBetween(start, end uint) *TimestampSetFilter`
   集合中元素值必须在 start 和 end 之间 （&gt;=start且&lt;=end）


#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimestampSetFilter) AddValidator(validator TimestampSetValidator) *TimestampSetFilter

验证函数类型如下：

	type TimestampSetValidator func(paramName string, paramValue []uint) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error