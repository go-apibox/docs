数据模型
=========

数据模型，即Model，它对数据库进行抽象，搭配 xorm 对数据进行快捷操作。

## Model 定义

示例：

	type Channel struct {
		ChannelId   uint `api:"rand" xorm:"pk"`
		ChannelName string
		ChannelKey  string `api:"randstr:32"`
		Status      string `api:"hidden"`
		CreateTime  uint32 `api:"createtime"`
	}

Model 的具体定义方法请参考 xorm 文档。

APIBox 引入了以下特殊标记，所有标记都需要包含在 api 分组中。
多个标记使用空格分隔，如果标记带有参数，则标记与参数使用:分隔，多个参数使用,分隔。

 - `rand`
   在 Create() 时生成随机数字。
   后面可带上两个参数：
    - 第一个参数：`uint`、`uint32`、`uint64` 或 `dateprefix`，不带参数则默认为 `uint32`。
    - 第二个参数：由两个数值组成，如100000,999999表示生成100000到999999间的数值（dateprefix无效）。
   当参数值为 `uint64` 时，生成的数值最大为 9007199254740992（这是 Javascript 所能表示的最大值）。
   当参数值为 `dateprefix` 时，将生成以当前日期为前缀的随机数，如：2015011574281383。
   
 - `randstr`
   在 Create() 时生成随机的字符串，字符集为：`0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`。
   后面可带两个参数：
    - 第一个参数，表示要生成的字符串长度，不带参数则默认为 `16`
    - 第二个参数，表示是否转为大小写，大写：upper，小写：lower。

 - `hidden`
   在 Detail() 或 List() 时要隐藏的字段。
   后面可带上一个参数，取值为：`*`、`list`、`detail`，不带则默认为 `*`。
   `*`：表示在 Detail() 或 List() 结果中均隐藏该字段
   `list`：表示仅在 List() 结果中隐藏该字段
   `detail`：表示仅在 List() 结果中隐藏该字段

 - `createtime`
   在 Create() 时会自动生成 Unix 时间戳的字段。

 - `updatetime`
   在 Create() 和 Update() 时会自动生成 Unix 时间戳的字段。

 - `deletetime`
   在 Delete() 时会自动生成 Unix 时间戳的字段。
   如果有这种标记的字段存在，则执行软删除，即不真正删除记录，则是将记录的该字段值设置为删除时间。

 - `showindex`
   表明该列用于自定义记录顺序。
   后面可带上一个参数，取值为："insert"、"append"。
   `insert` 表示在 Create() 时使用0作为排序序号，表中所有其它记录的排序序号全部自动+1。
   `append` 表示在 Create() 时自动取该表中该列最大的值+1作为排序序号。

## Model 注册

Model 定义后，需要注册到应用中才能使用，使用以下两个接口进行注册：

	func (app *App) RegisterModel(model interface{})
	func (app *App) RegisterModels(models []interface{})

示例：

	func main() {
		app, err := api.NewApp()
		if err != nil {
			println(err.Error())
			os.Exit(-1)
		}
	
		app.RegisterModel(Channel{})
		app.Run(apiRoutes)
	}

## dbop 接口参考

### Create()

接口声明：

	func Create(c *Context, bean interface{}, params *Params) interface{}

bean 可以为表名，也可以为指向结构的指针，如果为指针，则执行后结构变量将被填充新插入的值。

如果生成的 ID 在数据表中已存在，则会重新生成随机数，重试 3 次后仍存在，则返回错误。

### SessionCreate()

接口声明：

	func SessionCreate(c *Context, bean interface{}, params *Params, session *xorm.Session) interface{}

在指定 session 中执行操作，其它参数与 Create() 一致。

### Update()

接口声明：

	func Update(c *Context, bean interface{}, params *Params) interface{}

bean 可以为表名，也可以为指向结构的指针，如果 Model 定义中存在有 `updatetime` 标记的字段，则将被自动填充。
该操作要求 params 中必须包含主键字段。

### SessionUpdate()

接口声明：

	func SessionUpdate(c *Context, bean interface{}, params *Params, session *xorm.Session) interface{}

在指定 session 中执行操作，其它参数与 Update() 一致。

### UpdateEx()

接口声明：

	func UpdateEx(c *Context, bean interface{}, params *Params, querySettings map[string]string) interface{}

参数与 Update() 一致。

 - querySettings 定义了查询时要使用的具体条件方法。
	格式如：

		querySetting := map[string]string{
			"Balance": "expr:`balance`-$",
		}

	上述查询将被转化为：

		UPDATE ... SET `balance`=`balance`-$, ...

	目前支持的条件方法：
  	- `expr`
	  在对应字段上作表达式查询，这里条件方法 `expr` 参数中的 `$` 将被替换为 `params` 中对应变量的值。

### SessionUpdateEx()

接口声明：

	func SessionUpdateEx(c *Context, bean interface{}, params *Params, session *xorm.Session, querySettings map[string]string) interface{}
	
在指定 session 中执行操作，其它参数与 UpdateEx() 一致。

### Move()

接口声明：

	func Move(c *Context, bean interface{}, srcIndex, dstIndex uint32) interface{}

bean 可以为表名，也可以为指向结构的指针。
srcIndex 和 dstIndex 为源记录和目标记录的顺序编号。
如果 dstIndex > srcIndex，表示将源记录往后移插入到目标记录之后。
如果 dstIndex < srcIndex，表示将源记录往前移插入到目标记录之前。

### SessionMove()

接口声明：

	func SessionUpdate(c *Context, bean interface{}, srcIndex, dstIndex uint32, session *xorm.Session) interface{}

在指定 session 中执行操作，其它参数与 Move() 一致。

### Delete()

接口声明：

	func Delete(c *Context, bean interface{}, params *Params) interface{}

bean 可以为表名，也可以为指向结构的指针，如果 Model 定义中存在有 `deletetime` 标记的字段，则将执行软删除。
该操作要求 params 中必须包含主键字段。

### SessionDelete()

接口声明：

	func SessionDelete(c *Context, bean interface{}, params *Params, session *xorm.Session) interface{}

在指定 session 中执行操作，其它参数与 Delete() 一致。

### Detail()

接口声明：

	func Detail(c *Context, bean interface{}, params *Params) interface{}

接口说明见 DetailJoin()。

### DetailJoin()

接口声明：

	func DetailJoin(c *Context, bean interface{}, params *Params, joinCond [][]string) interface{}

该操作要求 params 中必须包含主键字段。

 - bean 可以为表名，也可以为指向结构的指针，如果为指针，则执行后结构变量将被填充查询到的值。
 - joinCond 为联表条件，示例参数值：

		[][]string{
			[]string{"INNER", "user", "user.user_id=post.user_id"},
		}

### List()

接口声明：

	func List(c *Context, beans interface{}, params *Params, querySettings map[string]string) interface{}

接口说明见 ListJoin()。

### ListJoin()

接口声明：

	func List(c *Context, beans interface{}, params *Params, querySettings map[string]string, joinCond [][]string) interface{}

 - beans 必须为指向结构 Slice 的指针。
 - params 中的值将被转化为查询条件。
   如果值为 filter.*Range 类型，则将转化为相应的区间查询操作。
  > 示例：
  > 请求参数：Age=[20,30)
  > 对应查询条件：age>=20 AND age<30
  如果值为 Slice 类型，则将转化为相应的 IN 查询操作。
  > 示例：
  > 请求参数：Status=normal,disabled
  > 对应查询条件：status IN ("normal", "disabled")

 - querySettings 定义了查询时要使用的具体条件方法。
	格式如：

		querySetting := map[string]string{
			"Name": "like:,%|table:user",
		}

	上述查询将被转化为：

		Where `user`.`name` LIKE ?

	目前支持的条件方法：
  	- `like`
	  将对应字段值作为 LIKE 查询。
	  后面可带参数，表示是否在模糊查询左右侧加上 %，如 `%,%` 将转化为 `%KEYWORD%`。
	  如不带参数，则默认为 `,%`。
  	- `table`
	  表示该字段属于哪个表。
	  在 Join 查询时，如果查询条件字段在多张表中都存在，则可能会出现以下错误：
	  `Error 1052: Column 'create_time' in order clause is ambiguous`
	  这时可以通过配置该字段的表名来解决，如 `table:account`。
  	- `or`
	  表示该参数对应的是OR查询。
	  后面带的参数表示要进行OR查询的字段列表。
	  示例：

			map[string]string{
				"Keyword": "or:ServerName,ServerIp",
			}

	  查询条件参数 Keyword 的值将自动作为 ServerName 和 ServerIp 字段的条件值。
	  支持在查询时指定表名，示例：

			map[string]string{
				":table:": "table:my_table_name",
			}


 - joinCond 为联表条件，示例参数值：

		[][]string{
			[]string{"INNER", "user", "user.user_id=post.user_id"},
		}


## dbop 操作示例

使用此类接口能够大幅减少编码量，可参见以下对比示例。

#### 使用前

	func ChannelCreateAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelName", filter.Required(), filter.String().MinLen(2).MaxLen(20))
		params.Add("ChannelKey", filter.String().MinLen(16).MaxLen(32))
		params.Add("Status", filter.Default("normal"), filter.String().In([]string{"normal", "disabled"}))
		if err := params.Parse(); err != nil {
			return err
		}
	
		channel := new(Channel)
		channel.ChannelId = api.RandUint()
		channel.ChannelName = params.GetString("ChannelName")
		channel.Status = params.GetString("Status")
		channel.CreateTime = api.Timestamp()
	
		if params.Has("ChannelKey") {
			channel.ChannelKey = params.GetString("ChannelKey")
		} else {
			channel.ChannelKey = api.RandStringN(16)
		}
	
		db, err := c.DB.GetMysql("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "DBNotExist").SetMessage(err.Error())
		}
		_, err = db.Insert(channel)
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "InsertFailed").SetMessage(err.Error())
		}
	
		return api.Combine("ChannelId", channel.ChannelId)
	}
	
	func ChannelUpdateAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Required(), filter.Uint())
		params.Add("ChannelName", filter.String().MinLen(2).MaxLen(20))
		params.Add("ChannelKey", filter.String().MinLen(16).MaxLen(32))
		params.Add("Status", filter.String().In([]string{"normal", "disabled"}))
		if err := params.Parse(); err != nil {
			return err
		}
	
		channel := new(Channel)
		channel.ChannelId = params.GetUint("ChannelId")
		channel.ChannelName = params.GetString("ChannelName")
		channel.ChannelKey = params.GetString("ChannelKey")
		channel.Status = params.GetString("Status")
	
		db, err := c.DB.GetMysql("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "DBNotExist").SetMessage(err.Error())
		}
		_, err = db.Update(channel)
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "UpdateFailed").SetMessage(err.Error())
		}
	
		return nil
	}
	
	func ChannelDetailAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Required(), filter.Uint())
		if err := params.Parse(); err != nil {
			return err
		}
	
		channel := new(Channel)
		channel.ChannelId = params.GetUint("ChannelId")
	
		db, err := c.DB.GetMysql("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "DBNotExist").SetMessage(err.Error())
		}
		has, err := db.Get(channel)
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "GetFailed").SetMessage(err.Error())
		}
		if !has {
			return c.Error.New(api.ErrorObjectNotExist, "Channel")
		}
	
		return api.Combine("Channel", channel)
	}
	
	func ChannelListAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Uint())
		params.Add("Status", filter.String().In([]string{"normal", "disabled"}))
		params.Add("CreateTime", filter.TimestampRange())
		if err := params.Parse(); err != nil {
			return err
		}
	
		channels := make([]Channel, 0)
	
		db, err := c.DB.GetMysql("default")
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "DBNotExist").SetMessage(err.Error())
		}
	
		if params.Has("ChannelId") {
			db.Where("channel_id", params.GetUint("ChannelId"))
		}
		if params.Has("Status") {
			db.Where("status", params.GetString("Status"))
		}
		// 此处仍未完整实现
		db.Limit(20, 10)
		err = db.Find(&channels)
		if err != nil {
			return c.Error.New(api.ErrorInternalError, "QueryFailed").SetMessage(err.Error())
		}
	
		return api.Combine("Channels", channels)
	}
	
#### 使用后

	func ChannelCreateAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelName", filter.Required(), filter.String().MinLen(2).MaxLen(20))
		params.Add("ChannelKey", filter.String().MinLen(16).MaxLen(32))
		params.Add("Status", filter.Default("normal"), filter.String().In([]string{"normal", "disabled"}))
		if err := params.Parse(); err != nil {
			return err
		}
	
		return api.Create(c, "Channel", params)
	}
	
	func ChannelUpdateAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Required(), filter.Uint())
		params.Add("ChannelName", filter.String().MinLen(2).MaxLen(20))
		params.Add("ChannelKey", filter.String().MinLen(16).MaxLen(32))
		params.Add("Status", filter.String().In([]string{"normal", "disabled"}))
		if err := params.Parse(); err != nil {
			return err
		}
	
		return api.Update(c, "Channel", params)
	}
	
	func ChannelDeleteAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Required(), filter.Uint())
		if err := params.Parse(); err != nil {
			return err
		}
	
		return api.Delete(c, "Channel", params)
	}
	
	func ChannelDetailAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.Add("ChannelId", filter.Required(), filter.Uint())
		if err := params.Parse(); err != nil {
			return err
		}
	
		return api.Detail(c, "Channel", params)
	}
	
	func ChannelListAction(c *api.Context) interface{} {
		params := c.NewParams()
		params.AddPagination().AddOrderBy("CreateTime", "desc", []string{"CreateTime"}, false)
		params.Add("ChannelId", filter.Uint())
		params.Add("ChannelName", filter.String())
		params.Add("Status", filter.StringSet().ItemIn([]string{"normal", "disabled"}))
		params.Add("CreateTime", filter.TimestampRange())
		if err := params.Parse(); err != nil {
			return err
		}
	
		channels := make([]Channel, 0)
		return api.List(c, &channels, params, map[string]string{
			"ChannelName": "like",
		})
	}
