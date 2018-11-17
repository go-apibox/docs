StringSet
========

StringSet 过滤器用于对字符串集合进行过滤。

#### 新建过滤器：

	ssFilter := filter.StringSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - string：如："hilyjiang,apibox,quyun"
 - []string：如：[]string{"hilyjiang", "apibox", "quyun"}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *StringSetFilter) Allow(vals ...string) *StringSetFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *StringSetFilter) KeepCase() *StringSetFilter`
   保持大小写（默认）

 - `func (f *StringSetFilter) ToLower() *StringSetFilter`
   转换为小写

 - `func (f *StringSetFilter) ToUpper() *StringSetFilter`
   转换为大写

 - `func (f *StringSetFilter) Delimiter(delimiter string) *StringSetFilter`
   指定集合分隔符，默认为：`,`。

 - `func (f *StringSetFilter) MinCount(count int) *StringSetFilter`
   集合允许的最少元素数量。

 - `func (f *StringSetFilter) MaxCount(count int) *StringSetFilter`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *StringSetFilter) ItemMinLen(length int) *StringSetFilter`
   集合中的字符串长度不能小于length（&gt;=length）
 
 - `func (f *StringSetFilter) ItemMaxLen(length int) *StringSetFilter`
   集合中的字符串长度不能大于length（&lt;=length）

 - `func (f *StringSetFilter) ItemShorterThan(length int) *StringSetFilter`
   集合中的字符串长度必须小于length（&lt;length）

 - `func (f *StringSetFilter) ItemLongerThan(length int) *StringSetFilter`
   集合中的字符串长度必须大于length（&gt;length）

 - `func (f *StringSetFilter) ItemBetween(minLength, maxLength int) *StringSetFilter`
   集合中的字符串长度必须在minLength和maxlength之间 （&gt;=minLength且&lt;=maxLength）

 - `func (f *StringSetFilter) ItemMatch(pattern string) *StringSetFilter`
   集合中的字符串必须匹配由pattern指定的正规表达式

 - `func (f *StringSetFilter) ItemIsNumeric() *StringSetFilter`
   集合中的字符串必须是数值，如：123、0x16、0777、1e10

 - `func (f *StringSetFilter) ItemIsDigit() *StringSetFilter`
   集合中的字符串必须全部由数字构成

 - `func (f *StringSetFilter) ItemIsAlpha() *StringSetFilter`
   集合中的字符串必须全部由字母构成

 - `func (f *StringSetFilter) ItemIsAlphaNumeric() *StringSetFilter`
   集合中的字符串必须全部由字母或数字构成

 - `func (f *StringSetFilter) ItemIn(set []string) *StringSetFilter`
   集合中的字符串必须是指定字符串集合中的一个成员

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *StringSetFilter) AddValidator(validator StringValidator) *StringSetFilter

验证函数类型如下：

	type StringValidator func(paramName string, paramValue []string) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error