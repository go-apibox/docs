开发步骤
========

利用 APIBox 提供的两个子框架进行系统开发的一般步骤举例：

### 一、API 系统开发

后端开发人员使用 `apibox/api` 实现 API 接口：
		
	http://api.example.com/?api_action=User.Info

### 二、Web 系统开发

#### 后端开发：

使用 `apibox/web` 实现：

 - 实现用户登录、注销

   用户登录成功后，将登录信息写入 Session，用于后续验证。
   
 - 实现 API 代理请求

   Web 系统使用 `/api` 路径用于将请求代理到 API 系统：

		http://example.com/api?api_action=User.Info
	   
	应用中设置：
	 - 代理：到 `http://example.com/api` 的所有请求都代理到 `http://api.example.com/`。
	 - CSRF：启用对 `http://example.com/api` 请求的 CSRF 防护。
	 - Session：设置未登录不允许访问的 API 接口列表。

#### 前端开发：

前端开发人员利用后端开发人员提供的环境，编写页面访问路径及模板配置文件，前端采用 AngularJS/ReactJS 框架驱动业务，通过 `/api` 调用后端 API 实现业务逻辑。