Default
======

Default 过滤器用于设置参数的默认值，当参数未传入时，使用默认值作为参数值。

#### 新建过滤器：

	defaultFilter := filter.Default("DefaultValue")

`Default()` 声明如下：

	func Default(defaultVal interface{}) *DefaultFilter

可以接受任意类型的值作为默认值。

#### 过滤器设置：

不支持。

#### 验证规则：

不支持。

#### 自定义验证：

不支持。