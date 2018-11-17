Time
======

Time 过滤器用于对时间进行过滤。

默认要求传入的参数的时间字符串格式必须为：2006-01-02。
可以通过过滤器设置 `HasTime()` 和 `Layout()` 对时间格式进行调整。

#### 新建过滤器：

	dtFilter := filter.Time()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："2015-01-01"
 - `time.Time`：如：time.Now()
 - `*time.Time`：如：&time.Now()

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimeFilter) Allow(vals ...string) *TimeFilter`
   允许参数值为指定列表中的字符串。
  
 - `func (f *TimeFilter) HasTime() *TimeFilter`
   时间中必须带有时间，如：2006-01-02 15:04:05

 - `func (f *TimeFilter) Layout(layout string) *TimeFilter`：
   时间格式，默认为：2006-01-02

#### 验证规则：

 - `func (f *TimeFilter) StartFrom(tm uint) *TimeFilter`
   时间不能早于 tm（&gt;=tm）
 
 - `func (f *TimeFilter) EndTo(tm uint) *TimeFilter`
   时间不能晚于 tm（&lt;=tm）

 - `func (f *TimeFilter) Before(tm uint) *TimeFilter`
   时间必须晚于 tm（&lt;tm）

 - `func (f *TimeFilter) After(tm uint) *TimeFilter`
   时间必须晚于 tm（&gt;tm）

 - `func (f *TimeFilter) Equal(tm uint) *TimeFilter`
   时间必须等于 tm（=tm）

 - `func (f *TimeFilter) Between(startTime, endTime uint) *TimeFilter`
   时间必须在 start 和 end 之间 （&gt;=start且&lt;=end）

#### 自定义验证：

如要需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimeFilter) AddValidator(validator TimeValidator) *TimeFilter

验证函数类型如下：

	type TimeValidator func(paramName string, paramValue *time.Time) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error