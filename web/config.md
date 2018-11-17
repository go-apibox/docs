Web 配置
========

### 配置格式

 - `web`

    - `debug`
      是否在终端输出调试信息，可选值：`true` 或 `false`，若不配置则默认为 `false`。

    - `base_url`
      处理该 web 服务的请求基路径，即请求以该路径为前缀，若不配置则默认为 `/`。

    - `static_setting`
      静态文件处理全局设置配置段。

      - `max_age`
        静态文件缓存时间，单位为秒，若不配置则默认为 `3600*24*365`，即365天。

    - `statics`
      静态文件配置列表。
      
      - `{路径匹配规则}`
        静态文件匹配的路径规则，必须以 `/` 开始。
        支持使用 {} 匹配一个url段，支持正则表达式，示例：
	        "/product/{id}"
	        "/{noname:(?:scripts|images|styles)}/"
	        "/favicon.ico"
        
         - `root`
           静态文件根目录，仅支持相对路径，如："." 表示当前路径。
           
         - `max_age`
            静态文件缓存时间，单位为秒，若不配置则使用全局设置中的 `max-age` 值。
           
         - `image_resize_enabled`
            是否启用图片大小缩放功能，默认为 false。
			启用后访问 image_60x60.png 将会自动生成 image.png 对应的 60x60 大小的图片。
           
         - `image_resize_sizes`
            允许自动生成的图片尺寸列表，如：
	            image_resize_sizes:
	            - 60x60
	            - 30x30
	            - 16x16
 
          - `mask_alias` 
            遮罩路径，如果遮罩路径存在，则使用遮罩路径作为别名路径。

    - `page_setting`
      模板页面全局设置配置段。

       - `base_dir`
         模板页面根路径，必须使用相对路径。
         
       - `global`
         全局配置项配置段。

         - `headers`
           指定HTTP响应的HTTP头内容，格式为：
           `{HTTP头}: {值}`
         
         - `perm`
           页面权限，可选值为：
             `public`（不需要登录就可以访问）
             `protected`（需要登录才能访问）
           如不配置则默认为 `public`。
         
         - `disallow_spiders`
           是否禁止搜索引擎收录，可选值为：
             `true`（禁止）
             `false`（允许）
           如不配置则默认为 `false`，允许搜索引擎收录。

         - `unauthed_redirect`
           未登录的情况下，是否跳转到登录页面。可选值：`true`/`false`，默认为 `true`。
           如果为 `false`，未登录时访问页面将返回 `401 page not authorized` 错误。

         - `unauthed_redirect_method`
           未登录跳转使用的方法，可选值：`http`/`javascript`，默认为 `http`。
           `http` 表示使用 http 302 进行跳转；`javascript` 表示使用 javascript 进行跳转控制。
         
         - `session_auth_key`
           用于判断是否已登录的 session 值。格式为：`{session名称}.{session变量名}`。
           默认为：`default.authed`。
         
         - `login_url`
           未登录时，要跳转到的URL地址。默认为：`/login?from={$FROM_URL$}`。
           `{$FROM_URL$}` 是个变量，将会替换为来源URL地址。

       - `inject`
         数据注入设置段。

          - `data`
            要注入的数据列表，格式如：
            `"version": "1.0"`
            每行一条数据，如不设置则默认为空列表。

          - `include_data`
            通过其它文件注入的数据列表，格式如：
            `"version": "components/version"`

          - `session`
            要注入的 session，格式为：
		    `{模板中的变量名}: {session名称}`
		    如不设置则默认为：`"session": "default"`，即将名为 default 的 Session 注入到模板变量 session 中。
		    
   - `pages`
       模板页面列表。
         
      - `{路径匹配规则}`
        页面路径，必须以 `/` 开头。
        支持使用 {} 匹配一个url段，支持正则表达式，示例：
	      `"/products/{key}/"`
	      `"/articles/{category}/{id:[0-9]+}"`
	        
	       - `tmpl`
	         模板文件路径，相对于 `base_dir` 的路径。
	         类库定义了下面几个特殊路径：
	         `@login`：表示该路径为登录操作（仅模拟登录，生产环境下请勿使用）。
	         `@logout`：表示该路径为注销登录操作。
	         `@restart`：表示该路径为重启操作。
	         `@captcha`：
		         表示该路径为验证码图片，需要传入请求参数 `id`，如：
			         `/captcha?id=lrLuuaWqlnrwgq`
		         如果要重新载入图片，则传入请求参数 `reload`，取值任意，如：
				     `/captcha?id=lrLuuaWqlnrwgq&reload=1648956411`
				 还可以通过 `width` 和 `height` 参数指定验证码图片的尺寸，如：
					 `/captcha?id=lrLuuaWqlnrwgq&width=120&height=40`

           - `data`
             要注入模板的数据列表。
             类库定义了一个针对 `tmpl` 为 `@login` 和 `@logout` 的路径的特殊**数据键** "@jump_to"，表示路径访问页面后要跳转的路径。
             另外，在访问 `tmpl` 为 `@login` 的路径时，data 中的数据列表将全部写到 session 中（@开头的除外）。
             当 `tmpl` 为 `@captcha` 时，`data` 中支持以下特殊**数据键**：

              - `@width`：验证码宽度，默认为 180。
              - `@height`：验证码高度，默认为 60。

             当 `tmpl` 为 `@restart` 时，`data` 中支持以下特殊**数据键**：

              - `@session_csrf_token`：存储 csrf token 的 session 名称及键名，默认为：`default.csrf_token`。

             类库对所有普通模板定义了以下特殊**数据值**：

              - `@captcha`：用于产生验证码ID，可通过参数指定长度，默认为6位。示例：`@captcha:4`。

           - `headers`
             指定HTTP响应的HTTP头内容，格式为：
             `{HTTP头}: {值}`

           - `perm`
             页面访问权限，默认取值为 `web.page_settings.global.perm` 配置值。

           - `disallow_spiders`
             是否禁止搜索引擎收录，默认取值为 `web.page_settings.global.disallow_spiders` 配置值。

           - `unauthed_redirect`
             未登录是否跳转到登录页面，默认取值为 `web.page_settings.global.unauthed_redirect` 配置值。

           - `unauthed_redirect_method`
             未登录跳转使用的方法，默认取值为 `web.page_settings.global.unauthed_redirect_method` 配置值。
               
           - `session_auth_key`
             用于判断是否已登录的 session 值。格式为：`{session名称}.{session变量名}`。
             默认取值为 `web.page_settings.global.session_auth_key` 配置值。
         
           - `login_url`
             未登录时，要跳转到的URL地址。
             默认取值为 `web.page_settings.global.login_url` 配置值。
          
           - `excepts`
             `perm` 配置的页面访问权限要排除的例外页面规则，如：

					- "/articles/{category}/{id:[0-9]+}":
						tmpl: "/articles/{$category$}/{$id$}.html"
						perm: public
						excepts:
						category:
						- "api/interface/side-menu.html"
						- "api/interface/interface-gparams.html"

   - `apis`
      API列表。

     - `{API访问路径}`

       - `server`
         API网关地址，URL格式，如：http://127.0.0.1:8080/。
       
       - `addr`
         API网关的网络地址，如：127.0.0.1:8080，/tmp/api.sock。
         定义后请求时将以此地址作为请求的网络地址。
         一般用于避免 DNS 查询。
         
       - `params`
         默认请求参数列表。

       - `sign_key`
         用于对请求进行签名的密钥。

       - `nonce_length`
         请求中随机字符串长度，用于防止网络重攻击。

       - `proxy_session`
          Session 代理设置。

         - `enabled`
           是否从 API 网关代理 session（即将 API 网关的 session 值复制到此）。
           默认为 `true`。
         
         - `session_map`
           Session 名称映射规则列表。
           当 API 网关返回多个 Session 时，该映射规则将把远程 API 的 Session 名映射为本地的 Session 名。
           格式为：`{远程session名}: {本地session名}`，默认值为：`"default": "default"`。
		   若 API 网关返回的 Session 不在规则列表中定义，则将被删除。

         - `encrypt_key_action`
           从 API 网关获取加密密钥的接口名称，默认为：`APIBox.Session.GetKey`。

         - `sign_key`
           调用获取加密密钥的接口时用于对 API 请求进行签名的密钥，如不设置则默认为 API 网关全局配置的 `sign_key` 值。

         - `nonce_length`
           调用获取加密密钥的接口时，请求中随机字符串长度，用于防止网络重攻击。
		   如不设置则默认为 API 网关全局配置的 `nonce_length` 值。

       - `captcha_actions`
         需要进行验证码检测的 API 接口列表及验证规则。支持通配符 `*`。
		 格式如：
		 
			captcha_actions:
			  {接口名称}:
			    identifier: {用于计数接口调用失败次数的参数名}
			    max_fail_count: {接口调用失败多少次后启用验证码}

         调用在该列表中的 API 接口时，需要传入以下两个参数进行验证码验证：
         
          - `captcha_id`：验证码的编号
          - `captcha_code`：验证码图形上的值

         如果验证失败，将返回类似如下错误：
	         
			{
			    "ACTION": "SMS.Send",
			    "CODE": "InvalidCaptcha",
			    "MESSAGE": "Captcha verified failed!"
			}


### 配置示例

	web:
	  debug: true
	  base_url: "/"
	  static_setting:
	    max_age: 0
	  statics:
	  - "/static/":
	      root: "."
	  - "/styles/":
	      root: "."
	  - "/scripts/":
	      root: "."
	      max_age: 10
	  page_setting:
	    base_dir: "views/"
	    global:
          headers:
            X-TEST: "test"
	      perm: public
	      unauthed_redirect: true
	      session_auth_key: licenseapi.authed
	      login_url: /login
	    inject:
	      data:
	        version: "1.0"
        include_data
          version: "components/version"
	      session:
	        session: licenseapi
	  pages:
	  - "/logout":
	      tmpl: "@logout"
	      data:
	        "@jump_to": /login
	        user_id: hilyjiang
	  - "/logintest":
	      tmpl: "@login"
	      data:
	        "@jump_to": /member
	        userid: hilyjiang
	  - "/login":
	      tmpl: login-main.html
          headers:
            Content-Type: "text/xhtml; charset=utf-8"
	      data:
	        "captcha_id": "@captcha:4"
	  - "/captcha":
	      tmpl: "@captcha"
	  - "/member":
	      tmpl: member.html
	      unauthed_redirect: false
	      perm: protected
	  - "/partials/{rest:.+}":
	      tmpl: "partials/{$rest$}"
	      perm: public
	      unauthed_redirect: false
	      excepts:
	        rest:
	        - "api/interface/side-menu.html"
	        - "api/interface/interface-gparams.html"
	  apis:
	  - "/api":
	      server: http://test.lj.quyun.net:8080/
	      addr: 192.168.122.120:8080
	      params:
	        api_debug: "1"
	      proxy_session:
	        enabled: true
	        session_map:
	          default: licenseapi
	        encrypt_key_action: APIBox.Session.GetKey
	        sign_key: abcdefg
	      captcha_actions:
	        User.Login:
              identifier: Username
              max_fail_count: 3

**使用验证码的模板示例**

	<body>
	<img src="/captcha?id={$captcha_id$}"
		style="cursor:pointer"
		title="点击更换验证码"
		onclick="this.src='/captcha?id={$captcha_id$}&reload='+Math.random()" />
	</body>