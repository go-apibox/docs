Email
======

Email 过滤器用于对邮箱进行过滤。

#### 新建过滤器：

	emFilter := filter.Email()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - string：如："hilyjiang@gmail.com"

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *EmailFilter) Allow(vals ...string) *EmailFilter`
   允许参数值为指定列表中的字符串。

 - `func (f *EmailFilter) KeepCase() *EmailFilter`
   保持大小写

 - `func (f *EmailFilter) ToLower() *EmailFilter`
   转换为小写（默认）

 - `func (f *EmailFilter) ToUpper() *EmailFilter`
    转换为大写

#### 验证规则：

 - `func (f *EmailFilter) MinLen(length int) *EmailFilter`
   邮箱长度不能小于length（&gt;=length）
 
 - `func (f *EmailFilter) MaxLen(length int) *EmailFilter`
   邮箱长度不能大于length（&lt;=length）

 - `func (f *EmailFilter) ShorterThan(length int) *EmailFilter`
   邮箱长度必须小于length（&lt;length）

 - `func (f *EmailFilter) LongerThan(length int) *EmailFilter`
   邮箱长度必须大于length（&gt;length）

 - `func (f *EmailFilter) Between(minLength, maxLength int) *StringFilter`
   邮箱长度必须在minLength和maxlength之间 （&gt;=minLength且&lt;=maxLength）

 - `func (f *EmailFilter) Domain(domain string) *EmailFilter`
   邮箱必须是以指定字符串为域

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *EmailValidator) AddValidator(validator StringValidator) *EmailValidator

验证函数类型如下：

	type EmailValidator func(paramName string, paramValue string) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error