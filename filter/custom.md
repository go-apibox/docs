自定义过滤器
=============

如果 filter 类库中提供的过滤器仍不能满足你的要求，可以实现自定义的过滤器。

每个过滤器都要实现以下接口：

	type Filter interface {
		Run(paramName string, paramValue interface{}) (interface{}, *Error)
	}

过滤器示例（Required过滤器）：

	package filter
	
	type RequiredFilter struct {
	}
	
	// Required return a require filter.
	func Required() *RequiredFilter {
		return &RequiredFilter{}
	}
	
	// Run make the filter running.
	func (f *RequiredFilter) Run(paramName string, paramValue interface{}) (interface{}, *Error) {
		if paramValue == nil {
			return nil, NewError(ErrorMissingParam, paramName)
		}
		return paramValue, nil
	}

错误时，通过 `NewError()` 返回错误：

	func NewError(errorType ErrorType, fields ...string) *Error

可使用的错误代码及对应的错误字符串：

	var appErrorCodes = map[ErrorType]string{
		ErrorObjectNotExist:   "ObjectNotExist",
		ErrorObjectDuplicated: "ObjectDuplicated",
		ErrorNoObjectUpdated:  "NoObjectUpdated",
		ErrorNoObjectDeleted:  "NoObjectDeleted",
		ErrorMissingParam:     "MissingParam",
		ErrorInvalidParam:     "InvalidParam",
		ErrorQuotaExceed:      "QuotaExceed",
		ErrorPermissionDenied: "PermissionDenied",
		ErrorOperationFailed:  "OperationFailed",
		ErrorInternalError:    "InternalError",
	}



