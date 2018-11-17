错误消息自动生成
================

## 实现目标

对 API 调用者而言，在 Action 中利用 `c.Error.New()` 返回 API 错误时，他们希望不仅能返回错误消息，并且能够指定错误消息的语言。

因此 APIBox 实现了错误消息自动生成机制，能在 API 输出结果中自动加上错误消息，并允许 API 调用者指定输出错误消息的语言。

APIBox 通过映射规则实现了在输出错误消息时根据请求参数中的 `api_lang` 输出指定语言的错误消息。

## 实现步骤

只要在请求参数中指定 `api_lang` 参数，即可输出对应语言的错误消息。

当前 `api_lang` 的可选值有：

 - `en_us`：英文
 - `zh_cn`：中文

## 错误消息转换规则

错误消息转换规则均由 APIBox 内置，不允许自定义。

转换规则可以分为两种：

- `静态转换规则`
- `动态转换规则`

### 静态转换规则

静态转换规则直接将错误码映射为对应的错误消息。

### 动态转换规则

有时候我们会遇到大量相似的错误码，并且这些错误码对应的错误消息也很相似，比如：

错误码为： `MissingParam:UserName` 、`MissingParam:Password`
错误消息为：“缺少用户名！”、“缺少密码！”

如果用静态转换规则来实现，则需要两个规则：

	"MissingParam:UserName": "缺少UserName！",
	"MissingParam:Password": "缺少Password！",

当类似的错误码越来越多时，使用静态转换规则来实现多语言就显得很麻烦了，这时我们可以使用动态转换规则来实现。

动态转换规则中定义了错误码与错误消息的转换规则，格式如下：

	"错误码匹配规则": "错误消息模板"

格式说明：

- 错误码匹配规则中可以使用通配符 `*` 来表示一个字段。
- 错误消息模板中，使用`{1}`、`{2}`、…`{N}`来表示错误码匹配规则中匹配到的字段。
- `{N:连接字符串}` 表示将匹配到的字段中的 `|` 号替换为连接字符串。

根据规则，上述示例可以用一条动态语言包转换规则来定义：

	"MissingParam:*": "缺少{1}！"

再来看一个更高级的用法：

	"MissingParam:*": "缺少参数{1:或}！"

按照该规则定义：：
- 错误码 `MissingParam:A` 将生成消息：“缺少参数A！”
- 错误码 `MissingParam:A|B` 将生成消息：“缺少参数A或B！”

### 内置规则参考

#### 全局转换规则

全局转换规则在此仅作参考，在 Action 中无法通过 `c.Error.New()` 来生成这些错误。

APIBox 内置的全局错误码的消息映射规则如下：

**(en_us)英文转换规则：**

| 错误码                       | 错误消息                             |
| --------------------------- | ------------------------------------|
| ActionNotExist              | API action does not exist!          |
| SystemMaintenance           | System is under maintenance!        |

**(zh_cn)中文转换规则：**

| 错误码                       | 错误消息                             |
| --------------------------- | ------------------------------------|
| ActionNotExist              | 接口不存在！                         |
| SystemMaintenance           | 系统维护中！                          |

#### 应用转换规则

在应用中可以通过 `c.Error.New()` 来生成以下应用级错误：

**(en_cn)英文转换规则：**

| 错误类型                   | 错误码/错误码模板              | 错误消息/错误消息模板                |
| ------------------------ | ---------------------------- | --------------------------------- |
| ErrorObjectNotExist      | ObjectNotExist               | Object does not exist!            |
|                          | ObjectNotExist:&#42;         | {1} does not exist!               |
| ErrorObjectDuplicated    | ObjectDuplicated             | Object duplicated!                |
|                          | ObjectDuplicated:&#42;       | {1} already exists!               |
|                          | ObjectDuplicated:&#42;:&#42; | {1} with same {2} already exists! |
| ErrorNoObjectUpdated     | NoObjectUpdated              | No object updated!                |
|                          | NoObjectUpdated:&#42;        | No {1} updated!                   |
| ErrorNoObjectDeleted     | NoObjectDeleted              | No object deleted!                |
|                          | NoObjectDeleted:*            | No {1} deleted!                   |
| ErrorMissingParam        | MissingParam:&#42;           | Missing param {1: or }!           |
| ErrorInvalidParam        | InvalidParam:&#42;           | Invalid param {1: or }!           |
|                          | InvalidParam:&#42;:&#42;     | Invalid param {1: or }: {2}!      |
| ErrorQuotaExceed         | QuotaExceed                  | Quota exceed!                     |
|                          | QuotaExceed:&#42;            | Quota exceed: {1}!                |
| ErrorPermissionDenied    | PermissionDenied             | Permission denied!                |
|                          | PermissionDenied:&#42;       | Permission denied: {1}!           |
| ErrorOperationFailed     | OperationFailed:&#42;        | Operation failed: {1}!            |
| ErrorInternalError       | InternalError:&#42;          | Internal error: {1}!              |
|                          | InternalError:&#42;:&#42;    | Internal error: {1} {2}!          |

**(zh_cn)中文转换规则：**

| 错误类型                   | 错误码/错误码模板              | 错误消息/错误消息模板                |
| ------------------------ | ---------------------------- | --------------------------------- |
| ErrorObjectNotExist      | ObjectNotExist               | 对象不存在！                        |
|                          | ObjectNotExist:&#42;         | {1}不存在！                        |
| ErrorObjectDuplicated    | ObjectDuplicated             | 对象已存在！                        |
|                          | ObjectDuplicated:&#42;       | {1}已存在！                        |
|                          | ObjectDuplicated:&#42;:&#42; | 相同{2}的{1}已存在！                |
| ErrorNoObjectUpdated     | NoObjectUpdated              | 没有对象被更新！                     |
|                          | NoObjectUpdated:&#42;        | 没有{1}被更新！                     |
| ErrorNoObjectDeleted     | NoObjectDeleted              | 没有对象被删除！                     |
|                          | NoObjectDeleted:*            | 没有{1}被删除！                     |
| ErrorMissingParam        | MissingParam:&#42;           | 缺少参数{1:或}！                    |
| ErrorInvalidParam        | InvalidParam:&#42;           | 无效的参数{1:或}！                  |
|                          | InvalidParam:&#42;:&#42;     | 无效的参数{1:或}：{2}！              |
| ErrorQuotaExceed         | QuotaExceed                  | 超出配额限制！                      |
|                          | QuotaExceed:&#42;            | 超出配额限制：{1}！                  |
| ErrorPermissionDenied    | PermissionDenied             | 没有权限！                          |
|                          | PermissionDenied:&#42;       | 没有权限：{1}！                     |
| ErrorOperationFailed     | OperationFailed:&#42;        | 操作失败：{1}！                     |
| ErrorInternalError       | InternalError:&#42;          | 服务器内部错误：{1}！                |
|                          | InternalError:&#42;:&#42;    | 服务器内部错误：{1}{2}！             |

## 词汇转换规则

当使用了动态转换规则后，我们发现输出的错误消息中仍然无法满足我们的需求，如上例中错误码对应的错误消息为：

	MissingParam:UserName: 缺少UserName！
	MissingParam:Password: 缺少Password！

我们希望将错误消息中的 `UserName` 和 `Password` 都替换为中文，这时只要在应用配置中定义好词汇转换规则就可以了：

	words:
	  en_us:
	    UserName: username
	    Password: password
	  zh_cn:
	    UserName: 用户名
	    Password: 密码

关于应用配置的详细信息，请参考：

 - [配置](config.md)
