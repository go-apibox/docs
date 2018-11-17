TimeRange
======================================

TimeRange 过滤器用于对时间区间参数进行过滤。

#### 新建过滤器：

	drFilter := filter.TimeRange()

该过滤器的 `Run()` 函数允许接收的参数值类型有：

 - `string`：如："[2014-01-01,2015-01-01)", "[2010-01-01,]", "(,2015-02-09]"
 - `types.TimeRange`：如：types.TimeRange{&time.Unix(0, 0), &time.Now(), true, false}
 - `*types.TimeRange`：如：&types.TimeRange{&time.Unix(0, 0), &time.Now(), true, false}

#### 过滤器设置：

以下过滤器设置会在验证前对参数值进行转换：

 - `func (f *TimeRangeValidator) Allow(vals ...string) *TimeRangeValidator`
   允许参数值为指定列表中的字符串。
  
 - `func (f *TimeRangeValidator) HasTime() *TimeRangeValidator`
   时间中必须带有时间，如：2006-01-02 15:04:05

 - `func (f *TimeRangeValidator) Layout(layout string) *TimeRangeValidator`
   时间格式，默认为：2006-01-02

 - `func (f *TimeRangeValidator) LeftDefault(tm string) *TimeRangeValidator`
   指定区间左值的默认值。当左值留空时，使用该值作为左值。

 - `func (f *TimeRangeValidator) RightDefault(tm string) *TimeRangeValidator`
   指定区间右值的默认值。当右值留空时，使用该值作为右值。

#### 验证规则：

 - `func (f *TimeRangeValidator) LeftStartFrom(tm string) *TimeRangeValidator`
   区间左值不能小于 Time（&gt;=Time）

 - `func (f *TimeRangeValidator) LeftEndTo(tm string) *TimeRangeValidator`
   区间左值不能大于 Time（&lt;=Time）

 - `func (f *TimeRangeValidator) LeftAfter(tm string) *TimeRangeValidator`
   区间左值必须大于 Time（&lt;Time）

 - `func (f *TimeRangeValidator) LeftBefore(tm string) *TimeRangeValidator`
   区间左值必须小于 Time（&gt;Time）

 - `func (f *TimeRangeValidator) LeftEqual(tm string) *TimeRangeValidator`
   区间左值必须等于 Time（=Time）

 - `func (f *TimeRangeValidator) LeftBetween(startTime, endTime string) *TimeRangeValidator`
   区间左值必须在 startTime 和 endTime 之间 （&gt;=startTime且&lt;=endTime）

 - `func (f *TimeRangeValidator) RightStartFrom(tm string) *TimeRangeValidator`
   区间右值不能小于 Time（&gt;=Time）

 - `func (f *TimeRangeValidator) RightEndTo(tm string) *TimeRangeValidator`
   区间右值不能大于 Time（&lt;=Time）

 - `func (f *TimeRangeValidator) RightAfter(tm string) *TimeRangeValidator`
   区间右值必须大于 Time（&lt;Time）

 - `func (f *TimeRangeValidator) RightBefore(tm string) *TimeRangeValidator`
   区间右值必须小于 Time（&gt;Time）

 - `func (f *TimeRangeValidator) RightEqual(tm string) *TimeRangeValidator`
   区间右值必须等于 Time（=Time）

 - `func (f *TimeRangeValidator) RightBetween(startTime, endTime string) *TimeRangeValidator`
   区间右值必须在 startTime 和 endTime 之间 （&gt;=startTime且&lt;=endTime）

 - `func (f *TimeRangeValidator) MinDistance(duration string) *TimeRangeValidator`
   区间左右值间隔时间不能短于 duration，duration支持的单位："s", "m", "h"，如："1h2m"

 - `func (f *TimeRangeValidator) MaxDistance(duration string) *TimeRangeValidator`
   区间左右值间隔时间不能长于 duration，duration支持的单位："s", "m", "h"，如："1h2m"

#### 自定义验证：

如果需要增加其它自定义验证规则，可以在过滤器上调用 `AddValidator` 来添加验证规则：

	func (f *TimeRangeValidator) AddValidator(validator TimeRangeValidator) *TimeRangeValidator

验证函数类型如下：

	type TimeRangeValidator func(paramName string, paramValue *types.TimeRange) *Error

如果验证成功，则返回 nil，否则使用 `filter.NewError` 来返回 `*Error` 错误：

	func NewError(errorType ErrorType, fields ...string) *Error