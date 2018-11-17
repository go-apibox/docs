String
======

String 过滤器用于对字符串进行过滤。

#### 新建过滤器：

	strFilter := filter.String()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - string：如："hilyjiang"

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *StringFilter) Allow(vals ...string) *StringFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *StringFilter) KeepCase() *StringFilter`
   保持大小写（默认）

 - `func (f *StringFilter) ToLower() *StringFilter`
   转换为小写

 - `func (f *StringFilter) ToUpper() *StringFilter`
   转换为大写

#### 验证规则：

 - `func (f *StringFilter) Length(length int) *StringFilter`
   字符串长度必须等于length（=length）

 - `func (f *StringFilter) MinLen(length int) *StringFilter`
   字符串长度不能小于length（&gt;=length）
 
 - `func (f *StringFilter) MaxLen(length int) *StringFilter`
   字符串长度不能大于length（&lt;=length）

 - `func (f *StringFilter) ShorterThan(length int) *StringFilter`
   字符串长度必须小于length（&lt;length）

 - `func (f *StringFilter) LongerThan(length int) *StringFilter`
   字符串长度必须大于length（&gt;length）

 - `func (f *StringFilter) Between(minLength, maxLength int) *StringFilter`
   字符串长度必须在minLength和maxlength之间 （&gt;=minLength且&lt;=maxLength）

 - `func (f *StringFilter) Match(pattern string) *StringFilter`
   字符串必须匹配由pattern指定的正规表达式

 - `func (f *StringFilter) IsNumeric() *StringFilter`
   字符串必须是数值，如：123、0x16、0777、1e10

 - `func (f *StringFilter) IsDigit() *StringFilter`
   字符串必须全部由数字构成

 - `func (f *StringFilter) IsAlpha() *StringFilter`
   字符串必须全部由字母构成

 - `func (f *StringFilter) IsAlphaNumeric() *StringFilter`
   字符串必须全部由字母或数字构成

 - `func (f *StringFilter) In(set []string) *StringFilter`
   字符串必须是指定字符串集合中的一个成员

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *StringFilter) AddValidator(validator StringValidator) *StringFilter

验证函数类型如下：

	type StringValidator func(paramName string, paramValue string) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error