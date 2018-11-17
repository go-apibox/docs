Int/Int32/Int64/Uint/Uint32/Uint64
===============================

Int/Int32/Int64/Uint/Uint32/Uint64 过滤器用于对整型参数进行过滤。
这几个过滤器具备相同的验证规则，可根据整型值的不同使用不同的过滤器类型。

#### 新建过滤器：

	intFilter := filter.Int()
	int32Filter := filter.Int32()
	int64Filter := filter.Int64()
	uintFilter := filter.Uint()
	uint32Filter := filter.Uint32()
	uint64Filter := filter.Uint64()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："123"

Int 过滤器还允许接收以下参数值类型：

 - `int`：如：123、-123

Int32 过滤器还允许接收以下参数值类型：

 - `int32`：如：int32(123)、int32(-123)

Int64 过滤器还允许接收以下参数值类型：

 - `int64`：如：int64(123)

Uint 过滤器还允许接收以下参数值类型：

 - `uint`：如：uint(123)

Uint32 过滤器还允许接收以下参数值类型：

 - `uint32`：如：uint32(123)

Uint64 过滤器还允许接收以下参数值类型：

 - `int64`：如：int64(123)

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *IntFilter) Allow(vals ...string) *IntFilter`
 - `func (f *Int32Filter) Allow(vals ...string) *Int32Filter`
 - `func (f *Int64Filter) Allow(vals ...string) *Int64Filter`
 - `func (f *UintFilter) Allow(vals ...string) *UintFilter`
 - `func (f *Uint32Filter) Allow(vals ...string) *Uint32Filter`
 - `func (f *Uint64Filter) Allow(vals ...string) *Uint64Filter`
   允许参数值为指定列表中的字符串。
&nbsp;
 - `func (f *IntFilter) Base(base int) *IntFilter`
 - `func (f *Int32Filter) Base(base int) *Int32Filter`
 - `func (f *Int64Filter) Base(base int) *Int64Filter`
 - `func (f *UintFilter) Base(base int) *UintFilter`
 - `func (f *Uint32Filter) Base(base int) *Uint32Filter`
 - `func (f *Uint64Filter) Base(base int) *Uint64Filter`
   指定数值的进制，如：2表示2进制、10表示10进制，默认为10。

#### 验证规则：

 - `func (f *IntFilter) Min(val int) *IntFilter`
 - `func (f *Int32Filter) Min(val int32) *Int32Filter`
 - `func (f *Int64Filter) Min(val int64) *Int64Filter`
 - `func (f *UintFilter) Min(val uint) *UintFilter`
 - `func (f *Uint32Filter) Min(val uint32) *Uint32Filter`
 - `func (f *Uint64Filter) Min(val uint64) *Uint64Filter`
   整数值不能小于 val（&gt;=val）
&nbsp;
 - `func (f *IntFilter) Max(val int) *IntFilter`
 - `func (f *Int32Filter) Max(val int32) *Int32Filter`
 - `func (f *Int64Filter) Max(val int64) *Int64Filter`
 - `func (f *UintFilter) Max(val uint) *UintFilter`
 - `func (f *Uint32Filter) Max(val uint32) *Uint32Filter`
 - `func (f *Uint64Filter) Max(val uint64) *Uint64Filter`
   整数值不能大于 val（&lt;=val）
&nbsp;
 - `func (f *IntFilter) LargerThan(val int) *IntFilter`
 - `func (f *Int32Filter) LargerThan(val int32) *Int32Filter`
 - `func (f *Int64Filter) LargerThan(val int64) *Int64Filter`
 - `func (f *UintFilter) LargerThan(val uint) *UintFilter`
 - `func (f *Uint32Filter) LargerThan(val uint32) *Uint32Filter`
 - `func (f *Uint64Filter) LargerThan(val uint64) *Uint64Filter`
   整数值必须大于 val（&lt;val）
&nbsp;
 - `func (f *IntFilter) SmallerThan(val int) *IntFilter`
 - `func (f *Int32Filter) SmallerThan(val int32) *Int32Filter`
 - `func (f *Int64Filter) SmallerThan(val int64) *Int64Filter`
 - `func (f *UintFilter) SmallerThan(val uint) *UintFilter`
 - `func (f *Uint32Filter) SmallerThan(val uint32) *Uint32Filter`
 - `func (f *Uint64Filter) SmallerThan(val uint64) *Uint64Filter`
   整数值必须小于 val（&gt;val）
&nbsp;
 - `func (f *IntFilter) Equal(val int) *IntFilter`
 - `func (f *Int32Filter) Equal(val int32) *Int32Filter`
 - `func (f *Int64Filter) Equal(val int64) *Int64Filter`
 - `func (f *UintFilter) Equal(val uint) *UintFilter`
 - `func (f *Uint32Filter) Equal(val uint32) *Uint32Filter`
 - `func (f *Uint64Filter) Equal(val uint64) *Uint64Filter`
   整数值必须等于 val（=val）
&nbsp;
 - `func (f *IntFilter) Between(min, max int) *IntFilter`
 - `func (f *Int32Filter) Between(min, max int32) *Int32Filter`
 - `func (f *Int64Filter) Between(min, max int64) *Int64Filter`
 - `func (f *UintFilter) Between(min, max uint) *UintFilter`
 - `func (f *Uint32Filter) Between(min, max uint32) *Uint32Filter`
 - `func (f *Uint64Filter) Between(min, max uint64) *Uint64Filter`
   整数值必须在 min 和 max 之间 （&gt;=min且&lt;=max）
&nbsp;
 - `func (f *IntFilter) In(set []int) *IntFilter`
 - `func (f *Int32Filter) In(set []int32) *Int32Filter`
 - `func (f *Int64Filter) In(set []int64) *Int64Filter`
 - `func (f *UintFilter) In(set []uint) *UintFilter`
 - `func (f *Uint32Filter) In(set []uint32) *Uint32Filter`
 - `func (f *Uint64Filter) In(set []uint64) *Uint64Filter`
   整数值必须是指定整数值集合中的一个成员

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *IntFilter) AddValidator(validator IntValidator) *IntFilter
	func (f *Int32Filter) AddValidator(validator Int32Validator) *Int32Filter
	func (f *Int64Filter) AddValidator(validator Int64Validator) *Int64Filter 
	func (f *UintFilter) AddValidator(validator UintValidator) *UintFilter 
	func (f *Uint32Filter) AddValidator(validator Uint32Validator) *Uint32Filter 
	func (f *Uint64Filter) AddValidator(validator Uint64Validator) *Uint64Filter 

验证函数类型如下：

	type IntValidator func(paramName string, paramValue int) *Error
	type Int32Validator func(paramName string, paramValue int32) *Error
	type Int64Validator func(paramName string, paramValue int64) *Error
	type UintValidator func(paramName string, paramValue uint) *Error
	type Uint32Validator func(paramName string, paramValue uint32) *Error
	type UintValidator func(paramName string, paramValue uint64) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error