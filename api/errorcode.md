错误码格式
============

APIBox 对 API 输出的错误码有严格要求：

- 错误码只能由字母和数字构成。
- 错误码必须以大写字母开头，错误由由多个单词构成的，每个单词首字母大写。
- 错误码有多个字段组成的，各个字段之间以冒号“:”进行分隔。

严格的错误码格式要求是 APIBox 实现将错误码自动生成为错误消息的基础。

## 全局错误

全局错误是由 APIBox 输出的错误，这些错误无法在应用中使用：

| 错误码                 | 错误消息                         | 错误描述       |
| --------------------- | ------------------------------- | ------------- |
| ActionNotExist        | API action does not exist!      | 接口不存在！    |
| SystemMaintenance     | System is under maintenance!    | 系统维护中！    |

## 应用错误

应用的 Action 中可通过 `c.Error.New()` 生成一个错误。

生成的应用错误码有三种格式：

 - [错误类型]
 - [错误类型]:[错误描述]
 - [错误类型]:[对象名称]:[错误描述]

多个对象名称之间支持使用 `|` 号分隔，如：`MissingParam:UserName|Password`。

APIBox 不允许应用自定义错误类型，但允许自定义 `[对象名称]` 和 `[错误描述]`，通过参数 `fields ...string` 指定。

以下是 APIBox 允许使用的错误类型及格式：

| 错误类型	               | 错误码格式                                  | 错误描述                   |
| ------------------------ | ----------------------------------------- | --------------------------|
| ErrorObjectNotExist      | ObjectNotExist<br>ObjectNotExist:[对象名称] | 对象不存在。|
| ErrorObjectDuplicated	  | ObjectDuplicated<br>ObjectDuplicated:[对象名称]<br>ObjectDuplicated:[对象名称]:[冲突字段]                          | 对象已存在，重复了。 |
| ErrorNoObjectUpdated	  | NoObjectUpdated<br>NoObjectUpdated:[对象名称] | 没有对象被更新。             |     
| ErrorNoObjectDeleted     | NoObjectDeleted<br>NoObjectDeleted:[对象名称] | 没有对象被删除。             |
| ErrorMissingParam        | MissingParam:[对象名称]| 缺少参数。|
| ErrorInvalidParam        | InvalidParam:[对象名称]<br>InvalidParam:[对象名称]:[错误描述]                    | 无效的参数。|
| ErrorQuotaExceed         | QuotaExceed<br>QuotaExceed:[错误描述] | 超出配额限制。 |
| ErrorPermissionDenied    | PermissionDenied<br>PermissionDenied:[错误描述] | 权限限制。 |
| ErrorOperationFailed     | OperationFailed:[错误描述] | 操作失败。<br>因其它业务限制导致的操作失败。|
| ErrorInternalError       | InternalError:[错误描述]<br>InternalError:[对象名称]:[错误描述] | 服务器内部错误。<br>因数据库异常、网络连接异常等原因导致的错误。|

## 错误消息自动生成

调用 `c.Error.New()` 时，并没有指定错误消息，但是在 API 输出结果中会自动加上错误消息。

这是由 APIBox 的错误消息自动生成机制实现的，详情请看：

 - [错误消息生成](errormsg.md)
 - [多语言支持](errorlang.md)
