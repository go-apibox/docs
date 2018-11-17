Json
======

Json 过滤器用于对 Json 字符串进行过滤。

#### 新建过滤器：

	jsonFilter := filter.Json()

该过滤器的 Run() 函数允许接收的参数值类型有：

 - string：如：&#96;{"Username": "hilyjiang"}&#96;

如果 json 分析成功，`Run()` 将返回一个 `interface{}` 类型的值。

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *JsonFilter) Allow(vals ...string) *JsonFilter`
   允许参数值为指定列表中的字符串。

以下过滤器设置会在验证后对参数值进行处理：

 - `func (f *JsonFilter) ToString() *JsonFilter`
   验证完后将参数值转换为字符串返回。

 - `func (f *JsonFilter) Output(outVar interface{}) *JsonFilter`
   将 json 解析结果输出到 outVar 变量中。

#### 验证规则：

不支持。

#### 自定义验证：

如需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *JsonFilter) AddValidator(validator JsonValidator) *JsonFilter

验证函数类型如下：

	type JsonValidator func(paramName string, paramValue interface{}) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error