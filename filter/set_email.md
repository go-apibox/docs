EmailSet
========

EmailSet 过滤器用于对邮箱地址集合进行过滤。

#### 新建过滤器：

	esFilter := filter.EmailSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - string：如："hilyjiang@gmail.com,hilyjiang@qq.com"
 - []string：如：[]string{"hilyjiang@gmail.com", "hilyjiang@qq.com"}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *EmailSetFilter) Allow(vals ...string) *EmailSetFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *EmailSetFilter) KeepCase() *EmailSetFilter`
   保持大小写

 - `func (f *EmailSetFilter) ToLower() *EmailSetFilter`
   转换为小写（默认）

 - `func (f *EmailSetFilter) ToUpper() *EmailSetFilter`
    转换为大写

 - `func (f *EmailSetFilter) Delimiter(delimiter string) *EmailSetFilter`
   指定集合分隔符，默认为：`,`。

 - `func (f *EmailSetFilter) MinCount(count int) *EmailSetFilter`
   集合允许的最少元素数量。

 - `func (f *EmailSetFilter) MaxCount(count int) *EmailSetFilter`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *EmailSetFilter) ItemMinLen(length int) *EmailSetFilter`
   集合中的邮箱地址长度不能小于length（&gt;=length）
 
 - `func (f *EmailSetFilter) ItemMaxLen(length int) *EmailSetFilter`
   集合中的邮箱地址长度不能大于length（&lt;=length）

 - `func (f *EmailSetFilter) ItemShorterThan(length int) *EmailSetFilter`
   集合中的邮箱地址长度必须小于length（&lt;length）

 - `func (f *EmailSetFilter) ItemLongerThan(length int) *EmailSetFilter`
   集合中的邮箱地址长度必须大于length（&gt;length）

 - `func (f *EmailSetFilter) ItemBetween(minLength, maxLength int) *EmailSetFilter`
   集合中的邮箱地址长度必须在minLength和maxlength之间 （&gt;=minLength且&lt;=maxLength）

 - `func (f *EmailSetFilter) ItemDomain(set []string) *EmailSetFilter`
   集合中的邮箱地址必须以指定字符串为域

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *EmailSetFilter) AddValidator(validator StringValidator) *EmailSetFilter

验证函数类型如下：

	type StringValidator func(paramName string, paramValue []string) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error