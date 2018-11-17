TimeSet
======================================

TimeSet 过滤器用于对时间区间参数进行过滤。

#### 新建过滤器：

	dsFilter := filter.TimeSet()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："2014-01-01,2015-01-01", "2015-02-09"
 - `[]string`：如：[]string{"2014-01-01", "2015-01-01"}
 - `[]time.Time`：如：[]time.Time{time.Unix(0, 0), time.Now()}
 - `[]*time.Time`：如：[]*time.Time{&time.Unix(0, 0), &time.Now()}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimeSetValidator) Allow(vals ...string) *TimeSetValidator`
   允许参数值为指定列表中的字符串。

 - `func (f *TimeSetValidator) Delimiter(delimiter string) *TimeSetValidator`
   指定区间分隔符，默认为：`,`。
   
 - `func (f *TimeSetValidator) HasTime() *TimeSetValidator`
   时间中必须带有时间，如：2006-01-02 15:04:05

 - `func (f *TimeSetValidator) Layout(layout string) *TimeSetValidator`
   时间格式，默认为：2006-01-02

 - `func (f *TimeSetValidator) MinCount(count int) *TimeSetValidator`
   集合允许的最少元素数量。

 - `func (f *TimeSetValidator) MaxCount(count int) *TimeSetValidator`
   集合允许的最多元素数量。

#### 验证规则：

 - `func (f *TimeSetValidator) ItemStartFrom(tm string) *TimeSetValidator`
  集合中元素值不能小于 tm（&gt;=tm）

 - `func (f *TimeSetValidator) ItemEndTo(tm string) *TimeSetValidator`
  集合中元素值不能大于 tm（&lt;=tm）

 - `func (f *TimeSetValidator) ItemAfter(tm string) *TimeSetValidator`
  集合中元素值必须大于 tm（&lt;tm）

 - `func (f *TimeSetValidator) ItemBefore(tm string) *TimeSetValidator`
  集合中元素值必须小于 tm（&gt;tm）

 - `func (f *TimeSetValidator) ItemBetween(startTime, endTime string) *TimeSetValidator`
  集合中元素值必须在 startTime 和 endTime 之间 （&gt;=startTime且&lt;=endTime）

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimeSetValidator) AddValidator(validator TimeSetValidator) *TimeSetValidator

验证函数类型如下：

	type TimeSetValidator func(paramName string, paramValue []*time.Time) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error