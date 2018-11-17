EmptyToNil
===========

EmptyToNil 过滤器会自动将空参数值转为nil。

#### 新建过滤器：

	e2nFilter := filter.EmptyToNil()

当传入的参数值为空字符串时，该参数会被删除（可认为未传递该参数）。