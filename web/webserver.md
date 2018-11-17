Web服务器
===========

webserver 在 web 类库的基础上实现了一个通用的 web 服务器。

建议将 webserver 安装到 `/usr/bin/webserver` 或 `/bin/webserver`，这样我们可以在任意目录下执行 webserver 命令。

该命令可指定参数 -c，用于指定配置文件的路径，如：

	webserver -c="/var/www/example.yaml"

如果不带任何参数，webserver 将会使用当前工作目录下的 config.yaml 作为配置文件。

webserver 使用配置文件中引用了 web 类库定义的配置格式，具体可参考：

 - [Web类库配置文件](config.md)

配置文件示例：

	app:
	  host: www.example.com
	  http_addr: :8080
	  tls:
	    enabled: false
      mirror_hosts:
      - example.com|:80
	
	web:
	  debug: true
	  static_setting:
	    max_age: 0
	  statics:
	  - "/static/":
	      root: "."
	  page_setting:
	    base_dir: "views/"
	    global:
	      perm: protected
	    inject:
	      data:
	        version: "1.0"
	  pages:
	  - "/logout":
	      tmpl: "@logout"
	      data:
	        "@jump_to": /login
	  - "/logintest":
	      tmpl: "@login"
	      perm: public
	      data:
	        "@jump_to": /member
	        user_id: hilyjiang
	        user_email: hilyjiang@gmail.com
	  - "/member":
	      tmpl: member.html
	      unauthed_redirect: true
	  - "/partials/{rest:.+}":
	      tmpl: "partials/{$rest$}"
	      unauthed_redirect: false
	      excepts:
	        rest:
	        - "aboutus.html"
	        - "help.html"
	  apis:
	  - "/api":
	      server: http://192.168.122.120:8080/
	      params:
	        api_debug: "1"
	        api_lang: "zh_cn"
	      proxy_session:
	        sign_key: "abcdefg"
