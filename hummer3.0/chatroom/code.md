# <!--错误码-->

| 错误码 | 错误信息                       | 描述                           |
| ------ | ------------------------------ | ------------------------------ |
| 0      | OK                             | 成功                           |
| 1      | UNKNOWN_ERROR                  | 未知错误                       |
| 100    | LOGIN_KICK_OUT                 | 您的帐号在别处登录，您被迫下线 |
| 101    | TERMINAL_LIMIT                 | 超出终端类限制，被其他端踢下线 |
| 102    | DEVICE_COUNT_LIMIT             | 超出设备数限制，被其他端踢下线 |
| 1000   | CLIENT_EXCEPTIONS              | 客户端错误                     |
| 1001   | UNINITIALIZED_EXCEPTION        | 没有初始化                     |
| 1002   | SDK_INVALID_PARAMETERS         | 参数有误                       |
| 1003   | DB_ERROR                       | 访问本地数据库错误             |
| 1004   | NETWORK_NOT_FOUND              | 网络不可达                     |
| 1005   | OPERATION_TIMEOUT              | 操作超时                       |
| 1006   | CONNECTION_TIMEOUT             | 网络连接超时                   |
| 1007   | CONNECTION_EXCEPTION           | 连接异常                       |
| 1008   | THROTTLING                     | 服务请求调用过于频繁           |
| 1009   | UNAUTHORIZED_EXCEPTION         | 鉴权失败                       |
| 1010   | THIRD_PARTY_SERVICE_ERROR      | 调用第三方服务发生错误         |
| 1011   | User not login                 | 用户未登录                     |
| 1011   | BAD_USER_ERROR                 | 未正确切换用户上下文           |
| 2000   | PROTOCOL_EXCEPTIONS            | 传输协议异常                   |
| 2001   | Invalid Operated Uid           | 无效uid                        |
| 2001   | Invalid Parameter              | 参数错误                       |
| 2001   | Invalid Parameter              | 通道信息长度超过上限           |
| 2001   | Invalid Roomid                 | 无效房间 ID                    |
| 2001   | Key Size Limit                 | 超过32个 Key                   |
| 2001   | Value Length Limit             | value 大于8kb                  |
| 2002   | INVALID_TOKEN_EXCEPTION        | Token校验错误                  |
| 2003   | EXPIRED_TOKEN_EXCEPTION        | Token过期                      |
| 2004   | RESOURCE_NOT_FOUND_EXCEPTION   | 资源不存在                     |
| 2005   | RESOURCE_ALREADY_EXIST         | 请求访问的资源已存在           |
| 2006   | LIMIT_EXCEEDED                 | 资源、关系数量超出了限定值     |
| 2007   | MSG_SIZE_LIMIT_EXCEEDED        | 消息长度超上限                 |
| 2008   | COUNT_LIMIT_EXCEEDED           | 数量超出了上限                 |
| 2018   | APP_ID_UNAVAILABLE             | 无效的appId                    |
| 3000   | User Permission Deny           | 操作者不是 owner或admin        |
| 3000   | PERMISSION_DENY                | 权限验证失败                   |
| 3001   | BLACK_LISTED                   | 用户被列入黑名单               |
| 3002   | TEMPORARILY_DENIED_EXCEPTION   | 暂时没有权限                   |
| 3003   | FORBIDDEN_EXCEPTION            | 操作被禁止                     |
| 3004   | USER_FORBIDDEN_EXCEPTION       | 用户操作被禁止                 |
| 3005   | BANNED_EXCEPTION               | 操作被封禁                     |
| 3006   | CHALLENGE_EXCEPTION            | 需要输入参数进行验证           |
| 3007   | CONTENT_CENSORSHIP_DENY        | 内容审查失败                   |
| 3011   | OPERATED_BY_MYSELF_DENY        | 不允许自己操作自己             |
| 4000   | Internal Server Error          | 内部调用错误                   |
| 4001   | SERVICE_UNAVAILABLE            | 暂时无法提供服务               |
| 4002   | BUSINESS_SERVER_ERROR          | 业务服务异常                   |
| 4003   | Operation Rate Limit           | 超出接口调用频率限制           |
| 4003   | OPERATION_TOO_OFTEN            | 操作过于频繁                   |
| 4004   | REPETITIVE_OPERATION           | 重复操作                       |
| 5001   | Chatroom Not Exist             | 聊天室不存在                   |
| 5003   | Operator User Not In Room      | 操作者不在聊天室内             |
| 5004   | OPERATED_USER_NOT_IN_CHAT_ROOM | 被操作者不在聊天室             |
| 5012   | CHAR_ROOM_USER_NOT_ADMIN       | 用户不是管理员                 |
| 5101   | User Not In Channel            | 用户不在通道                   |
| 5109   | Channel count Limit            | 通道数量超过上限               |
| 5110   | Already joined                 | 重复加入/正在加入              |
