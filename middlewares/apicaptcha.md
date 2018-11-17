API 验证码
============

### 实现原理

对一些需要验证的API，该中间件会要求调用时增加两个参数：
- `CaptchaId`: 验证码ID
- `CaptchaCode`: 验证码值

该中间件提供了2个API用于生成验证码：
- `GetCaptcha`: 生成验证码ID
- `ShowCaptcha`: 根据验证码ID生成图片，支持以下参数：
    + `Reload`：是否重新加载验证码
    + `Width`：验证码宽度
    + `Height`：验证码高度

### 相关配置

- apicaptcha：
   API 验证码中间件配置组

  - disabled:
    是否禁用该中间件，不配置则默认为：`false`。

  - get_action:
    生成验证码ID的API接口名称，默认为`GetCaptcha`。

  - show_action:
    显示验证码图片的API接口名称，默认为`ShowCaptcha`。

  - image_width:
    验证码图片默认宽度，默认为180。

  - image_height:
    验证码图片默认宽度，默认为60。

  - actions：
      接口匹配规则。

     - ACTIONNAME(接口名称)：
        接口白名单：要提供验证码的API接口名称列表，默认为空。支持通配符 `*`。
        + identifier: 接口调用失败时使用的计数键
        + max_fail_count: 失败几次后启用验证码，默认为0，表示立即启用。


该中间件会先检查接口是否在接口列表中，如果在，则再验证接口调用失败次数是否超过限制，超出时才要求提交验证码信息。


配置示例：

    apicaptcha:
      get_action: GetCaptcha
      show_action: ShowCaptcha
      image_width: 180
      image_height: 60
      actions:
        VerifyCode.SignUp.SendEmail:
          max_fail_count: 0
        VerifyCode.ResetPassword.SendEmail:
          max_fail_count: 0
        VerifyCode.ResetPassword.SendCellphone:
          max_fail_count: 0
        User.Login:
          identifier: Username
          max_fail_count: 3

### 相关错误

| 错误码                 | 错误消息                  | 错误描述         |
| --------------------- | -------------------------- | --------------- |
| MissingCaptcha        | Missing captcha!           | 缺少验证码！   |
| WrongCaptcha          | Wrong captcha!             | 验证码错误！    |

### 接口参考

- `func (cs *CSRF) Enable()`
  启用该中间件。

- `func (cs *CSRF) Disable()`
  禁用该中间件。

### 使用示例

**应用入口：**

    package main
    
    import (
    	"os"
    
    	"github.com/go-apibox/api"
    	"github.com/go-apibox/apicaptcha"
    )
    
    func main() {
    	app, err := api.NewApp()
    	if err != nil {
    		println(err.Error())
    		os.Exit(1)
    	}
    
    	app.Use(apicaptcha.NewCaptcha(app))
    	app.Run(apiRoutes)
