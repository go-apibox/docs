IntRange/Int32Range/Int64Range/UintRange/Uint32Range/Uint64Range
=============================================================

IntRange/Int32Range/Int64Range/UintRange/Uint32Range/Uint64Range 过滤器用于对整型区间参数进行过滤。
这几个过滤器具备相同的验证规则，可根据整型值的不同使用不同的过滤器类型。

#### 新建过滤器：

	irFilter := filter.IntRange()
	i32rFilter := filter.Int32Range()
	i64rFilter := filter.Int64Range()
	uirFilter := filter.UintRange()
	ui32rFilter := filter.Uint32Range()
	ui64rFilter := filter.Uint64Range()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："[0,100)"、"(,100]"、"[0,]"

IntRange 过滤器还允许接收以下参数值类型：

 - `types.IntRange`：如：types.IntRange{-123, 0, false, true}
 - `*types.IntRange`：如：&types.IntRange{-123, 0, false, true}

Int32Range 过滤器还允许接收以下参数值类型：

 - `types.Int32Range`：如：types.Int32Range{-123, 0, false, true}
 - `*types.Int32Range`：如：&types.Int32Range{-123, 0, false, true}

Int64Range 过滤器还允许接收以下参数值类型：

 - `types.Int64Range`：如：types.Int64Range{-123, 0, false, true}
 - `*types.Int64Range`：如：&types.Int64Range{-123, 0, false, true}

UintRange 过滤器还允许接收以下参数值类型：

 - `types.UintRange`：如：types.UintRange{0, 123, false, true}
 - `*types.UintRange`：如：&types.UintRange{0, 123, false, true}

Uint32Range 过滤器还允许接收以下参数值类型：

 - `types.Uint32Range`：如：types.Uint32Range{0, 123, false, true}
 - `*types.Uint32Range`：如：&types.Uint32Range{0, 123, false, true}

Uint64Range 过滤器还允许接收以下参数值类型：

 - `types.Uint64Range`：如：types.Uint64Range{0, 123, false, true}
 - `*types.Uint64Range`：如：&types.Uint64Range{0, 123, false, true}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *IntRangeFilter) Allow(vals ...string) *IntRangeFilter`
 - `func (f *Int32RangeFilter) Allow(vals ...string) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) Allow(vals ...string) *Int64RangeFilter`
 - `func (f *UintRangeFilter) Allow(vals ...string) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) Allow(vals ...string) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) Allow(vals ...string) *Uint64RangeFilter`
   允许参数值为指定列表中的字符串。
&nbsp;
 - `func (f *IntRangeFilter) LeftDefault(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftDefault(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftDefault(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftDefault(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftDefault(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftDefault(val uint64) *Uint64RangeFilter`
   指定区间左值的默认值。当左值留空时，使用该值作为左值。
&nbsp;
 - `func (f *IntRangeFilter) RightDefault(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightDefault(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightDefault(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightDefault(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightDefault(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightDefault(val uint64) *Uint64RangeFilter`
   指定区间右值的默认值。当右值留空时，使用该值作为右值。

#### 验证规则：

 - `func (f *IntRangeFilter) LeftMin(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftMin(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftMin(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftMin(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftMin(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftMin(val uint64) *Uint64RangeFilter`
   区间左值不能小于 val（&gt;=val）
&nbsp;
 - `func (f *IntRangeFilter) LeftMax(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftMax(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftMax(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftMax(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftMax(val uin32t) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftMax(val uint64) *Uint64RangeFilter`
   区间左值不能大于 val（&lt;=val）
&nbsp;
 - `func (f *IntRangeFilter) LeftLargerThan(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftLargerThan(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftLargerThan(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftLargerThan(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftLargerThan(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftLargerThan(val uint64) *Uint64RangeFilter`
   区间左值必须大于 val（&lt;val）
&nbsp;
 - `func (f *IntRangeFilter) LeftSmallerThan(val int) *IntRangeFilter`
 - `func (f *IntRangeFilter) LeftSmallerThan(val int) *IntRangeFilter`
 - `func (f *Int64RangeFilter) LeftSmallerThan(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftSmallerThan(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftSmallerThan(val uint32) *UintRangeFilter`
 - `func (f *Uint64RangeFilter) LeftSmallerThan(val uint64) *Uint64RangeFilter`
   区间左值必须小于 val（&gt;val）
&nbsp;
 - `func (f *IntRangeFilter) LeftEqual(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftEqual(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftEqual(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftEqual(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftEqual(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftEqual(val uint64) *Uint64RangeFilter`
   区间左值必须等于 val（=val）
&nbsp;
 - `func (f *IntRangeFilter) LeftBetween(min, max int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) LeftBetween(min, max int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) LeftBetween(min, max int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) LeftBetween(min, max uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) LeftBetween(min, max uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) LeftBetween(min, max uint64) *Uint64RangeFilter`
   区间左值必须在 min 和 max 之间 （&gt;=min且&lt;=max）
&nbsp;
 - `func (f *IntRangeFilter) RightMin(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightMin(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightMin(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightMin(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightMin(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightMin(val uint64) *Uint64RangeFilter`
   区间右值不能小于 val（&gt;=val）
&nbsp;
 - `func (f *IntRangeFilter) RightMax(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightMax(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightMax(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightMax(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightMax(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightMax(val uint64) *Uint64RangeFilter`
   区间右值不能大于 val（&lt;=val）
&nbsp;
 - `func (f *IntRangeFilter) RightLargerThan(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightLargerThan(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightLargerThan(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightLargerThan(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightLargerThan(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightLargerThan(val uint64) *Uint64RangeFilter`
   区间右值必须大于 val（&lt;val）
&nbsp;
 - `func (f *IntRangeFilter) RightSmallerThan(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightSmallerThan(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightSmallerThan(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightSmallerThan(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightSmallerThan(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightSmallerThan(val uint64) *Uint64RangeFilter`
   区间右值必须小于 val（&gt;val）
&nbsp;
 - `func (f *IntRangeFilter) RightEqual(val int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightEqual(val int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightEqual(val int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightEqual(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightEqual(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightEqual(val uint64) *Uint64RangeFilter`
   区间右值必须等于 val（=val）
&nbsp;
 - `func (f *IntRangeFilter) RightBetween(min, max int) *IntRangeFilter`
 - `func (f *Int32RangeFilter) RightBetween(min, max int32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) RightBetween(min, max int64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) RightBetween(min, max uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) RightBetween(min, max uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) RightBetween(min, max uint64) *Uint64RangeFilter`
   区间右值必须在 min 和 max 之间 （&gt;=min且&lt;=max）
&nbsp;
 - `func (f *IntRangeFilter) MinDistance(val uint) *IntRangeFilter`
 - `func (f *Int32RangeFilter) MinDistance(val uint32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) MinDistance(val uint64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) MinDistance(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) MinDistance(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) MinDistance(val uint64) *Uint64RangeFilter`
   区间左右值最小距离不能小于 val（>=val）
&nbsp;
 - `func (f *IntRangeFilter) MaxDistance(val uint) *IntRangeFilter`
 - `func (f *Int32RangeFilter) MaxDistance(val uint32) *Int32RangeFilter`
 - `func (f *Int64RangeFilter) MaxDistance(val uint64) *Int64RangeFilter`
 - `func (f *UintRangeFilter) MaxDistance(val uint) *UintRangeFilter`
 - `func (f *Uint32RangeFilter) MaxDistance(val uint32) *Uint32RangeFilter`
 - `func (f *Uint64RangeFilter) MaxDistance(val uint64) *Uint64RangeFilter`
   区间左右值最大距离不能大于 val（>=val）

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *IntRangeFilter) AddValidator(validator IntRangeValidator) *IntRangeFilter
	func (f *Int32RangeFilter) AddValidator(validator Int32RangeValidator) *Int32RangeFilter
	func (f *Int64RangeFilter) AddValidator(validator Int64RangeValidator) *Int64RangeFilter
	func (f *UintRangeFilter) AddValidator(validator UintRangeValidator) *UintRangeFilter
	func (f *Uint32RangeFilter) AddValidator(validator Uint32RangeValidator) *Uint32RangeFilter
	func (f *Uint64RangeFilter) AddValidator(validator Uint64RangeValidator) *Uint64RangeFilter

验证函数类型如下：

	type IntRangeValidator func(paramName string, paramValue *types.IntRange) *Error
	type Int32RangeValidator func(paramName string, paramValue *types.Int32Range) *Error
	type Int64RangeValidator func(paramName string, paramValue *types.Int64Range) *Error
	type UintRangeValidator func(paramName string, paramValue *types.UintRange) *Error
	type Uint32RangeValidator func(paramName string, paramValue *types.Uint32Range) *Error
	type Uint64RangeValidator func(paramName string, paramValue *types.Uint64Range) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error