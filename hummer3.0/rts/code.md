# 错误码

## ErrorCode

| 错误码 | 含义 |
| :--- | :--- |
| HMRErrorCodeSuccess(0) | 操作成功 |
| HMRErrorCodeUnknown(1) | 未知错误 |
| HMRErrorCodeLoginKickOut(100) | 登录互踢 |
| HMRErrorCodeUninitialized(1001) | 没有初始化 |
| HMRErrorCodeDB(1003) |  访问本地数据库错误 |
| HMRErrorCodeOperationTimeout(1005) | 操作超时 |
| HMRErrorCodeConnectionException(1007) | 连接异常 |
| HMRErrorCodeUnauthorized(1009) | 没有登录 |
| HMRErrorCodeBadUser(1011) | 如果业务没有正确处理用户上下文切换，例如业务已经注销了用户，但是没有调用Hummer.close，则会产生该错误 |
| HMRErrorCodeInvalidParameter(2001) | 参数验证错误 |
| HMRErrorCodeInvalidToken(2002) | Token校验错误 |
| HMRErrorCodeExpiredToken(2003) | Token过期 |
| HMRErrorCodeResourceNotFound(2004) | 资源、关系不存在 |
| HMRErrorCodeMsgSizeLimitExceeded(2007) | 消息长度超上限 |
| HMRErrorCodeAppidUnavailable(2018) | appid不可用 |
| HMRErrorCodePermissionDeny(3000) | 权限验证失败 |
| HMRErrorCodeContentCensorshipDeny(3007) | 内容审查失败 |
| HMRErrorCodeUserChatDisabled(3009) | 用户被禁言 |
| HMRErrorCodeInternalServerError(4000) | 服务器错误 |
| HMRErrorCodeOperationTooOften(4003) | 操作过于频繁 |
| HMRErrorCodeUserNotInRoom(5101) | 用户不在频道 |
| HMRErrorCodeRoomUserLimitExceeded(5102) | 频道人数超过上限 |
| HMRErrorCodeRoomUserInfoItemsOverFlow(5103) | 频道用户信息个数超过上限 |
| HMRErrorCodeRoomUserInfoSizeOverFlow(5104) | 频道用户信息长度超过上限 |
| HMRErrorCodeRoomInfoItemsOverFlow(5105) | 频道信息个数超过上限 |
| HMRErrorCodeRoomInfoSizeOverFlow(5106) | 频道信息长度超过上限 |
| HMRErrorCodeRoomPropsNotExist(5108) | 频属性不存在 |
| HMRErrorCodeRoomCountLimitExceeded(5109) | 请求频道数量超过上限 |
