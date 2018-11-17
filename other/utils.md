小工具
========

APIBox 将开发过程中经常会需要用到的一些函数整理到此：

## 随机

 - `func RandStringN(n int) string`
   生成指定长度的随机字符串。
   字符集合为：`0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`。

 - `func RandString() string`
   生成16位长度的随机字符串。

 - `func RandUint() uint`
   生成随机的 uint 整数。
 
 - `func RandUint32() uint32`
   生成随机的 uint32 整数。

 - `func RandUint64() uint64`
   生成随机的 uint64 整数。

 - `func RandDatePrefixUint64() uint64`
   生成基于日期的随机数，格式为：`YYYYMMDD########`，日期后面固定为8位数字。
   如：`2015011574281383`。

## 时间

 - `func Timestamp() uint32`
   返回当前的 Unix 时间戳。

## Map

 - `func Combine(key string, val interface{}) map[string]interface{}`
   合并 key 和 val，返回一个 map。

## 变量

 - `func CamelCase(str string) string`
   将变量转为大驼峰命名格式，如：hello_world => HelloWorld。

 - `func SnakeCase(str string) string`
   将变量转为蛇形命名格式，如：HelloWorld => hello_world。

## IP

 - `func IsPrivateIp(ip string) bool`
   验证 IP 地址是否为私有 IP，符合以下范围的均为私有IP：
    - 127.0.0.0 –  127.255.255.255
    - 10.0.0.0 –  10.255.255.255
    - 172.16.0.0 – 172. 31.255.255
    - 192.168.0.0 – 192.168.255.255

## 黑白名单匹配器

	func NewMatcher() *Matcher
	func (m *Matcher) SetWhiteList(whiteList []string) *Matcher
	func (m *Matcher) AddWhiteItem(item string) *Matcher
	func (m *Matcher) DelWhiteItem(item string) *Matcher
	func (m *Matcher) SetBlackList(blackList []string) *Matcher
	func (m *Matcher) AddBlackItem(item string) *Matcher
	func (m *Matcher) DelBlackItem(item string) *Matcher
	func (m *Matcher) Match(target string) bool

检查步骤：
先检查白名单，再检查黑名单。
如果白名单为空，则 Match 返回 false。
如果白名单为非空，只有匹配白名单并且不匹配黑名单的才会返回 true。

## 文件大小

	// Bytes(82854982) -> 83MB
	func Bytes(s uint64) string	
	
	// IBytes(82854982) -> 79MiB
	func IBytes(s uint64) string
	
	// HBytes(82854982) -> 79M
	func HBytes(s uint64) string
	
	// ParseBytes("42MB") -> 42000000, nil
	// ParseBytes("42mib") -> 44040192, nil
	func ParseBytes(s string) (uint64, error)

## 路径

	func AbsPath(path string) string
	
根据当前运行程序文件所在路径，返回相对于当前运行程序的绝对路径。