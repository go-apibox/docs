快速开始
==========

Web 开发过程中，经常需要对 HTTP 请求参数进行分析验证，在验证失败后，返回错误。这种验证规则通常具有很大的相似性，因此我们将它封装成类库——过滤器。

引入过滤器包：

	import "git.quyun.com/apibox/filter"

以下示例验证用户名是否合法，验证规则：

 - 用户名长度必须在6到10位
 - 用户名必须由字母和数字构成

另外，在验证前需要先将用户名转换为小写。

	package main
	
	import (
		"fmt"
	
		"git.quyun.com/apibox/filter"
	)
	
	func main() {
		f := filter.String().ToLower().Between(6, 10).IsAlphaNumeric()
	
		fmt.Println("- hily")
		if username, err := f.Run("Username", "hily"); err != nil {
			fmt.Println("Error:", err.Error())
		} else {
			fmt.Println("Username:", username)
		}
	
		fmt.Println("- hilyjiang520")
		if username, err := f.Run("Username", "hilyjiang520"); err != nil {
			fmt.Println("Error:", err.Error())
		} else {
			fmt.Println("Username:", username)
		}
	
		fmt.Println("- hily_jiang")
		if username, err := f.Run("Username", "hily_jiang"); err != nil {
			fmt.Println("Error:", err.Error())
		} else {
			fmt.Println("Username:", username)
		}
	
		fmt.Println("- hilyjiang")
		if username, err := f.Run("Username", "hilyjiang"); err != nil {
			fmt.Println("Error:", err.Error())
		} else {
			fmt.Println("Username:", username)
		}
	
		fmt.Println("- HILYJIANG")
		if username, err := f.Run("Username", "HILYJIANG"); err != nil {
			fmt.Println("Error:", err.Error())
		} else {
			fmt.Println("Username:", username)
		}
	}

输出如下：

	- hily
	Error: InvalidParam:Username:TooShort
	- hilyjiang520
	Error: InvalidParam:Username:TooLong
	- hily_jiang
	Error: InvalidParam:Username:NotAlphaNumeric
	- hilyjiang
	Username: hilyjiang
	- HILYJIANG
	Username: hilyjiang