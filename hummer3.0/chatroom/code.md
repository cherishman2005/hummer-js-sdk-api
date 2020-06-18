# 错误码

## Error.Code

| 错误码                            | 含义                                                         |
| :-------------------------------- | :----------------------------------------------------------- |
| Success(0)                        | 操作成功                                                     |
| ClientExceptions(1000)            | 版本非法                                                     |
| Uninitialized(1001)               | 没有初始化                                                   |
| InvalidParameters(1002)           | 调用参数非法                                                 |
| IOError(1003)                     | io错误                                                       |
| NetworkNotFound(1004)             | 无法找到网络                                                 |
| OperationTimeout(1005)            | 操作超时                                                     |
| ConnectionTimeout(1006)           | 连接超时                                                     |
| ConnectionFailed(1007)            | 连接异常                                                     |
| Throttling(1008)                  | 调用过于频繁                                                 |
| Unauthorized(1009)                | 没有登录                                                     |
| ThirdPartyServiceError(1010)      | 解析服务端数据出错                                           |
| BadUser(1011)                     | 如果业务没有正确处理用户上下文切换，例如业务已经注销了用户，但是没有调用Hummer.close，则会产生该错误 |
| ProtocolExceptions(2000)          | 网络协议编解码错误                                           |
| InvalidContent(2001)              | 参数验证错误                                                 |
| TokenInvalid(2002)                | Token校验错误                                                |
| TokenExpired(2003)                | Token过期                                                    |
| ResourceNotFound(2004)            | 资源、关系不存在                                             |
| ResourceAlreadyExist(2005)        | 资源、关系已存在                                             |
| LimitExceeded(2006)               | 资源、关系超上限                                             |
| MessageSizeLimitExceeded(2007)    | 消息长度超上限                                               |
| AccessDenied(3000)                | 没有权限                                                     |
| Blacklisted(3001)                 | 在黑名单中                                                   |
| kTemporarilyDeniedException(3002) | 暂时没有权限                                                 |
| kForbiddenException(3003)         | 操作被禁止                                                   |
| kUserForbiddenException(3004)     | 用户操作被禁止                                               |
| kBannedException(3005)            | 操作被封禁                                                   |
| kChallengeException(3006)         | 需要输入参数进行验证                                         |
| kInspectionFailedException(3007)  | 审查失败                                                     |
| InternalServerExceptions(4000)    | 服务器内部错误                                               |
| ServiceUnavailable(4001)          | 服务不可用                                                   |
| kBusinessServerError(4002)        | 业务服务器                                                   |
| kServiceThrottling(4003)          | 服务阻塞                                                     |
| UndefinedExceptions(-1)           | 其它未定义异常类型                                           |