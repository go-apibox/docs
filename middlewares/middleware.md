中间件
======

中间件为了实现某一特定功能，会在 HTTP 请求处理过程中透明地加入额外的处理逻辑。

在 [应用](app.md) 章节中，我们提到可以使用以下方式注册 HTTP 中间件：

	func (app *App) Use(middlewares ...negroni.Handler)

如果要自行实现中间件，请查看 negroni 相关文档：

 - [https://github.com/codegangsta/negroni](https://github.com/codegangsta/negroni)

APIBox 实现了以下中间件：

 - API签名验证：github.com/go-apibox/apisign
 - API重攻击防护：github.com/go-apibox/apinonce
 - API日志记录：github.com/go-apibox/apilog
 - API幂等性：github.com/go-apibox/apitoken
 - API代理：github.com/go-apibox/apiproxy
 - API Session 验证：github.com/go-apibox/apisession
 - API CSRF 防护：github.com/go-apibox/apicsrf
 - API总线：github.com/go-apibox/apibus
 - API初始化：github.com/go-apibox/apiinit
 - API连线检测：github.com/go-apibox/apiping

当中间件一同使用时，需要注意执行顺序：

1. HTTP请求
2. API初始化
3. API日志记录
4. API重攻击防护
5. API签名验证/API Session验证
6. API CSRF防护
7. API连线检测
8. API幂等性
9. API代理请求/API总线
10. HTTP请求处理

**中间件文档：**

 - [API签名验证](apisign.md)
 - [API重攻击防护](apinonce.md)
 - [API日志记录](apilog.md)
 - [API幂等性](apitoken.md)
 - [API代理请求](apiproxy.md)
 - [API Session 验证](apisession.md)
 - [API CSRF 防护](apicsrf.md)
 - [API总线](apibus.md)
 - [API初始化](apiinit.md)
 - [API连线检测](apiping.md)

