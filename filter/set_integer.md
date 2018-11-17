IntSet/Int32Set/Int64Set/UintSet/Uint32Set/Uint64Set
=====================================================

IntSet/Int32Set/Int64Set/UintSet/Uint32Set/Uint64Set 过滤器用于对整型集合参数进行过滤。
这几个过滤器具备相同的验证规则，可根据整型值的不同使用不同的过滤器类型。

#### 新建过滤器：

	isFilter := filter.IntSet()
	i32sFilter := filter.Int32Set()
	i64sFilter := filter.Int64Set()
	uisFilter := filter.UintSet()
	ui32sFilter := filter.Uint32Set()
	ui64sFilter := filter.Uint64Set()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："0,123,456"、"123"
 - `[]string`：如：[]string{"0", "123", "456"}

IntSet 过滤器还允许接收以下参数值类型：

 - `[]int`：如：[]int{0, 123, 456}、[]int{-456, -123, 0}

Int32Set 过滤器还允许接收以下参数值类型：

 - `[]int32`：如：[]int32{0, 123, 456}

Int64Set 过滤器还允许接收以下参数值类型：

 - `[]int64`：如：[]int64{0, 123, 456}

UintSet 过滤器还允许接收以下参数值类型：

 - `[]uint`：如：[]uint{0, 123, 456}

Uint32Set 过滤器还允许接收以下参数值类型：

 - `[]uint32`：如：[]uint32{0, 123, 456}

Uint64Set 过滤器还允许接收以下参数值类型：

 - `[]int64`：如：[]int64{0, 123, 456}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *IntSetFilter) Allow(vals ...string) *IntSetFilter`
 - `func (f *Int32SetFilter) Allow(vals ...string) *Int32SetFilter`
 - `func (f *Int64SetFilter) Allow(vals ...string) *Int64SetFilter`
 - `func (f *UintSetFilter) Allow(vals ...string) *UintSetFilter`
 - `func (f *Uint32SetFilter) Allow(vals ...string) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) Allow(vals ...string) *Uint64SetFilter`
   允许参数值为指定列表中的字符串。
&nbsp;
 - `func (f *IntSetFilter) Base(base int) *IntSetFilter`
 - `func (f *Int32SetFilter) Base(base int) *Int32SetFilter`
 - `func (f *Int64SetFilter) Base(base int) *Int64SetFilter`
 - `func (f *UintSetFilter) Base(base int) *UintSetFilter`
 - `func (f *Uint32SetFilter) Base(base int) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) Base(base int) *Uint64SetFilter`
   指定数值的进制，如：2表示2进制、10表示10进制，默认为10。
&nbsp;
 - `func (f *IntSetFilter) Delimiter(delimiter string) *IntSetFilter`
 - `func (f *Int32SetFilter) Delimiter(delimiter string) *Int32SetFilter`
 - `func (f *Int64SetFilter) Delimiter(delimiter string) *Int64SetFilter`
 - `func (f *UintSetFilter) Delimiter(delimiter string) *UintSetFilter`
 - `func (f *Uint32SetFilter) Delimiter(delimiter string) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) Delimiter(delimiter string) *Uint64SetFilter`
   指定集合分隔符，默认为：`,`。
&nbsp;
 - `func (f *IntSetFilter) MinCount(count int) *IntSetFilter`
 - `func (f *Int32SetFilter) MinCount(count int) *IntSet32Filter`
 - `func (f *Int64SetFilter) MinCount(count int) *Int64SetFilter`
 - `func (f *UintSetFilter) MinCount(count int) *UintSetFilter`
 - `func (f *Uint32SetFilter) MinCount(count int) *UintSet32Filter`
 - `func (f *Uint64SetFilter) MinCount(count int) *Uint64SetFilter`
   集合允许的最少元素数量。
&nbsp;
 - `func (f *IntSetFilter) MaxCount(count int) *IntSetFilter`
 - `func (f *Int32SetFilter) MaxCount(count int) *IntSet32Filter`
 - `func (f *Int64SetFilter) MaxCount(count int) *Int64SetFilter`
 - `func (f *UintSetFilter) MaxCount(count int) *UintSetFilter`
 - `func (f *Uint32SetFilter) MaxCount(count int) *UintSet32Filter`
 - `func (f *Uint64SetFilter) MaxCount(count int) *Uint64SetFilter`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *IntSetFilter) ItemMin(val int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemMin(val int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemMin(val int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemMin(val uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemMin(val uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemMin(val uint64) *Uint64SetFilter`
   集合中元素值不能小于 val（&gt;=val）
&nbsp;
 - `func (f *IntSetFilter) ItemMax(val int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemMax(val int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemMax(val int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemMax(val uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemMax(val uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemMax(val uint64) *Uint64SetFilter`
   集合左值不能大于 val（&lt;=val）
&nbsp;
 - `func (f *IntSetFilter) ItemLargerThan(val int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemLargerThan(val int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemLargerThan(val int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemLargerThan(val uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemLargerThan(val uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemLargerThan(val uint64) *Uint64SetFilter`
   集合中元素值必须大于 val（&lt;val）
&nbsp;
 - `func (f *IntSetFilter) ItemSmallerThan(val int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemSmallerThan(val int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemSmallerThan(val int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemSmallerThan(val uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemSmallerThan(val uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemSmallerThan(val uint64) *Uint64SetFilter`
   集合中元素值必须小于 val（&gt;val）
&nbsp;
 - `func (f *IntSetFilter) ItemBetween(min, max int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemBetween(min, max int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemBetween(min, max int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemBetween(min, max uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemBetween(min, max uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemBetween(min, max uint64) *Uint64SetFilter`
   集合中元素值必须在 min 和 max 之间 （&gt;=min且&lt;=max）
&nbsp;
 - `func (f *IntSetFilter) ItemIn(set []int) *IntSetFilter`
 - `func (f *Int32SetFilter) ItemIn(set []int32) *Int32SetFilter`
 - `func (f *Int64SetFilter) ItemIn(set []int64) *Int64SetFilter`
 - `func (f *UintSetFilter) ItemIn(set []uint) *UintSetFilter`
 - `func (f *Uint32SetFilter) ItemIn(set []uint32) *Uint32SetFilter`
 - `func (f *Uint64SetFilter) ItemIn(set []uint64) *Uint64SetFilter`
   集合中元素值必须在集合 set 中


#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *IntSetFilter) AddValidator(validator IntSetValidator) *IntSetFilter
	func (f *Int32SetFilter) AddValidator(validator Int32SetValidator) *Int32SetFilter
	func (f *Int64SetFilter) AddValidator(validator Int64SetValidator) *Int64SetFilter
	func (f *UintSetFilter) AddValidator(validator UintSetValidator) *UintSetFilter
	func (f *Uint32SetFilter) AddValidator(validator UintSetValidator) *Uint32SetFilter
	func (f *Uint64SetFilter) AddValidator(validator Uint64SetValidator) *Uint64SetFilter

验证函数类型如下：

	type IntSetValidator func(paramName string, paramValue []int) *Error
	type Int32SetValidator func(paramName string, paramValue []int32) *Error
	type Int64SetValidator func(paramName string, paramValue []int64) *Error
	type UintSetValidator func(paramName string, paramValue []uint) *Error
	type Uint32SetValidator func(paramName string, paramValue []uint32) *Error
	type Uint64SetValidator func(paramName string, paramValue []uint64) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error