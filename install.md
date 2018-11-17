安装
====


### Go 版本

APIBox 当前基于 go 1.4 版本构建，如果要基于 APIBox 构建应用，请使用 go 1.4。


### 安装 golib

APIBox 的安装依赖于 golib 工具，安装 golib（仅支持 linux 64位系统）：

    wget http://download.quyun.net/go/golib -O $GOPATH/bin/golib
    chmod +x $GOPATH/bin/golib

设置 golib 下载路径环境变量 GOLIB_PREFIX：

    cat > /etc/profile.d/go.sh <<EOF
    export GOROOT=\$HOME/go
    export GOPATH=\$HOME/godev
    export PATH=\$PATH:\$GOROOT/bin:\$GOPATH/bin
    export GOLIB_PREFIX=http://golib.quyun.net/
    EOF
    source /etc/profile


### 安装 APIBox 及依赖库

	# 依赖库
    golib get gopkg.in/yaml.v2
    golib get gopkg.in/fsnotify.v1
    golib get gopkg.in/flosch/pongo2.v3
    golib get github.com/codegangsta/cli
    golib get github.com/codegangsta/negroni
    golib get github.com/gorilla/mux
    golib get github.com/gorilla/context
	golib get github.com/gorilla/securecookie
	golib get github.com/gorilla/sessions
	golib get github.com/gorilla/websocket
    golib get github.com/go-sql-driver/mysql
    golib get github.com/go-xorm/core
    golib get github.com/go-xorm/xorm
    golib get github.com/mattn/go-sqlite3
    golib get github.com/bitly/go-simplejson
    golib get github.com/fatih/structs
    golib get github.com/streadway/simpleuuid
  
	# APIBox组件
    golib get git.quyun.com/apibox/api
    golib get git.quyun.com/apibox/types
    golib get git.quyun.com/apibox/config
    golib get git.quyun.com/apibox/cache
    golib get git.quyun.com/apibox/filter
	golib get git.quyun.com/apibox/session
	golib get git.quyun.com/apibox/utils
    golib get git.quyun.com/apibox/apisign
    golib get git.quyun.com/apibox/apinonce
    golib get git.quyun.com/apibox/apilog
    golib get git.quyun.com/apibox/apitoken
    golib get git.quyun.com/apibox/apiproxy
    golib get git.quyun.com/apibox/apisession
    golib get git.quyun.com/apibox/apicsrf
    golib get git.quyun.com/apibox/apiclient
    golib get git.quyun.com/apibox/web


### 更新 APIBox 及依赖库

更新 APIBox，只需重新执行上一节中的安装命令即可。