过滤器
=======

## 通用类型

filter 类库实现了三种通用过滤器：

 - [Default](default.md)
   当参数值为 `nil` 时，使用默认值。

 - [Required](required.md)
   必须传入参数值，否则参数验证失败。

 - [EmptyToNil](empty2nil.md)
   当参数值为空字符串时，当作 `nil` 值处理。
   
## 简单类型

filter 类库按最终需要的参数类型，实现了以下几种基本过滤器：

 - [String](string.md)
   参数值必须为字符串类型。

 - [Email](email.md)
   参数值必须为 Email 类型。
  
 - [Int/Int32/Int64/Uint/Uint32/Uint64](integer.md)
   参数值必须为整数类型。
  
 - [Float32/Float64](float.md)
   参数值必须为浮点数类型。
   
 - [Time](time.md)
   参数值必须为时间类型。

 - [Timestamp](timestamp.md)
   参数值必须为时间戳类型。
   
 - [IP](ip.md)
   参数值必须为 IP 类型。

 - [CIDR](cidr.md)
   参数值必须为 CIDR 类型。

 - [Json](json.md)
   参数值必须是 json 字符串类型。

## 区间类型

filter 类库实现以下几种区间过滤器：

 - [IntRange/Int32Range/Int64Range/UintRange/Uint32Range/Uint64Range](range_integer.md)
   参数值必须是整数区间。

 - [TimeRange](range_time.md)
   参数值必须为时间区间。

 - [TimestampRange](range_timestamp.md)
   参数值必须为时间戳区间。
   
## 集合类型

filter 类库实现以下几种集合过滤器：

 - [StringSet](set_string.md)
   参数值必须是字符串集合。

 - [EmailSet](set_email.md)
   参数值必须是邮箱地址集合。

 - [IntSet/Int32Set/Int64Set/UintSet/Uint32Set/Uint64Set](set_integer.md)
   参数值必须是整数集合。

 - [TimeSet](set_time.md)
   参数值必须是时间集合。

 - [TimestampSet](set_timestamp.md)
   参数值必须是时间戳集合。

 - [IPSet](set_ip.md)
   参数值必须是IP地址集合。

 - [CIDRSet](set_cidr.md)
   参数值必须是CIDR地址集合。

## 过滤器使用说明

过滤器使用基本步骤：

1. 新建过滤器
2. 设置过滤规则
3. 执行 `Run()` 函数对参数进行过滤

`Run()` 函数是每个过滤器都必须实现的函数，它会根据过滤器中设置的验证规则，逐个对参数值进行检测，当所有规则都验证成功时，最终返回对应类型的参数值。