Float32/Float64
===============================

Float32/Float64 过滤器用于对浮点数参数进行过滤。
这2个过滤器具备相同的验证规则，可根据浮点数的不同使用不同的过滤器类型。

#### 新建过滤器：

	float32Filter := filter.Float32()
    float64Filter := filter.Float64()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："1.25"

Float32 过滤器还允许接收以下参数值类型：

 - `float32`：如：float32(1.235)

Float64 过滤器还允许接收以下参数值类型：

 - `float64`：如：float64(1.23425235)

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *Float32Filter) Allow(vals ...string) *Float32Filter`
 - `func (f *Float64Filter) Allow(vals ...string) *Float64Filter`
   允许参数值为指定列表中的字符串。

#### 验证规则：

 - `func (f *Float32Filter) Min(val float32) *Float32Filter`
 - `func (f *Float64Filter) Min(val float64) *Float64Filter`
   浮点数不能小于 val（&gt;=val）
&nbsp;
 - `func (f *Float32Filter) Max(val float32) *Float32Filter`
 - `func (f *Float64Filter) Max(val float64) *Float64Filter`
   浮点数不能大于 val（&lt;=val）
&nbsp;
 - `func (f *Float32Filter) LargerThan(val float32) *Float32Filter`
 - `func (f *Float64Filter) LargerThan(val float64) *Float64Filter`
   浮点数必须大于 val（&lt;val）
&nbsp;
 - `func (f *Float32Filter) SmallerThan(val float32) *Float32Filter`
 - `func (f *Float64Filter) SmallerThan(val float64) *Float64Filter`
   浮点数必须小于 val（&gt;val）
&nbsp;
 - `func (f *Float32Filter) Equal(val float32) *Float32Filter`
 - `func (f *Float64Filter) Equal(val float64) *Float64Filter`
   浮点数必须等于 val（=val）
&nbsp;
 - `func (f *Float32Filter) Between(min, max float32) *Float32Filter`
 - `func (f *Float64Filter) Between(min, max float64) *Float64Filter`
   浮点数必须在 min 和 max 之间 （&gt;=min且&lt;=max）
&nbsp;
 - `func (f *Float32Filter) In(set []float32) *Float32Filter`
 - `func (f *Float64Filter) In(set []float64) *Float64Filter`
   浮点数必须是指定浮点数集合中的一个成员
&nbsp;
 - `func (f *Float32Filter) DecimalSpace(length int) *Float32Filter`
 - `func (f *Float64Filter) DecimalSpace(length int) *Float64Filter`
   浮点数的小数位数必须等于 length。

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *Float32Filter) AddValidator(validator Float32Validator) *Float32Filter
	func (f *Float64Filter) AddValidator(validator Float64Validator) *Float64Filter 

验证函数类型如下：

	type Float32Validator func(paramName string, paramValue float32) *Error
	type Float64Validator func(paramName string, paramValue float64) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error