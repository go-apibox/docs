TimestampRange
======================================

TimestampRange 过滤器用于对时间戳区间参数进行过滤。

#### 新建过滤器：

	trFilter := filter.TimestampRange()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："[0, 1419314831)", "(1419314831,]", "[,1419314831)"
 - `types.TimestampRange`：如：types.TimestampRange{0, 1419314831, true, false}
 - `*types.TimestampRange`：如：&types.TimestampRange{0, 1419314831, true, false}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimestampRangeFilter) Allow(vals ...string) *TimestampRangeFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *TimestampRangeFilter) LeftDefault(val uint32) *TimestampRangeFilter`
   指定区间左值的默认值。当左值留空时，使用该值作为左值。

 - `func (f *TimestampRangeFilter) RightDefault(val uint32) *TimestampRangeFilter`
   指定区间右值的默认值。当右值留空时，使用该值作为右值。

#### 验证规则：

 - `func (f *TimestampRangeFilter) LeftStartFrom(val uint32) *TimestampRangeFilter`
   区间左值不能小于 val（&gt;=val）

 - `func (f *TimestampRangeFilter) LeftEndTo(val uint32) *TimestampRangeFilter`
   区间左值不能大于 val（&lt;=val）

 - `func (f *TimestampRangeFilter) LeftAfter(val uint32) *TimestampRangeFilter`
   区间左值必须大于 val（&lt;val）

 - `func (f *TimestampRangeFilter) LeftBefore(val uint32) *TimestampRangeFilter`
   区间左值必须小于 val（&gt;val）

 - `func (f *TimestampRangeFilter) LeftEqual(val uint32) *TimestampRangeFilter`
   区间左值必须等于 val（=val）

 - `func (f *TimestampRangeFilter) LeftBetween(start, end uint) *TimestampRangeFilter`
   区间左值必须在 start 和 end 之间 （&gt;=start且&lt;=end）

 - `func (f *TimestampRangeFilter) RightStartFrom(val uint32) *TimestampRangeFilter`
   区间右值不能小于 val（&gt;=val）

 - `func (f *TimestampRangeFilter) RightEndTo(val uint32) *TimestampRangeFilter`
   区间右值不能大于 val（&lt;=val）

 - `func (f *TimestampRangeFilter) RightAfter(val uint32) *TimestampRangeFilter`
   区间右值必须大于 val（&lt;val）

 - `func (f *TimestampRangeFilter) RightBefore(val uint32) *TimestampRangeFilter`
   区间右值必须小于 val（&gt;val）

 - `func (f *TimestampRangeFilter) RightEqual(val uint32) *TimestampRangeFilter`
   区间右值必须等于 val（=val）

 - `func (f *TimestampRangeFilter) RightBetween(start, end uint) *TimestampRangeFilter`
   区间右值必须在 start 和 end 之间 （&gt;=start且&lt;=end）

 - `func (f *TimestampRangeFilter) MinDistance(val uint32) *TimestampRangeFilter`
   区间左右值最小距离不能小于 val（>=val）

 - `func (f *TimestampRangeFilter) MaxDistance(val uint32) *TimestampRangeFilter`
   区间左右值最大距离不能大于 val（>=val）

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimestampRangeFilter) AddValidator(validator TimestampRangeValidator) *TimestampRangeFilter

验证函数类型如下：

	type TimestampRangeValidator func(paramName string, paramValue *types.TimestampRange) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error