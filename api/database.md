数据库
=======

APIBox 的数据源管理目前封装了 [xorm](http://xorm.io/) 提供的 ORM 功能。

数据源管理器在应用的结构中定义：

	type App struct {
		Config *Config
		DB     *DbManager
	}

数据源管理器会自动载入应用配置中定义的数据源，当前支持 mysql 和 sqlite3 数据源。

在 Action 中可以通过 Context 来访问数据源管理器，如：

	dbm := c.App.DB

为简化开发，APIBox 直接在 Context 中定义了 DB 成员，因此可以直接使用以下方式访问数据源管理器：

	dbm := c.DB

数据源管理器提供的方法：

 - `func (dbm *DbManager) AddMysql(dbAlias, host, port, user, passwd, dbName string) error`
	添加 MySQL 数据源。

 - `func (dbm *DbManager) GetMysql(dbAlias string) (*xorm.Engine, error)`
   获取指定名称的 MySQL 数据源，返回结果为 `*xorm.Engine`，可直接使用 xorm 中定义的方法进行操作。

 - `func (dbm *DbManager) RemoveMysql(dbAlias string)`
   删除指定名称的 MySQL 数据源。 

 - `func (dbm *DbManager) AddSqlite3(dbAlias, sqliteDb string) error`
   添加 Sqlite3 数据源。

 - `func (dbm *DbManager) GetSqlite3(dbAlias string) (*xorm.Engine, error)`
   获取指定名称的 Sqlite3 数据源，返回结果为 `*xorm.Engine`，可直接使用 xorm 中定义的方法进行操作。

 - `func (dbm *DbManager) RemoveSqlite3(dbAlias string)`
   删除指定名称的 Sqlite3 数据源。 

当使用 MySQL 或 Sqlite3 时，需要加载相应驱动：

** MySQL **

	import (
	    _ "github.com/go-sql-driver/mysql"
	)

** Sqlite3 **

	import (
	    _ "github.com/mattn/go-sqlite3"
	)