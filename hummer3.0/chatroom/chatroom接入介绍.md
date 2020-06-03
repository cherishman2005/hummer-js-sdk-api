
# 聊天室ChatRoom接入流程

## 聊天室ChatRoom主要接入流程

### hummer对象
1. Hummer初始化
		hummer = Hummer.createHummer({appid});
		需要的入参包括：appid；

1. 登录
		hummer.login({uid, token});

		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852

1. 创建聊天室
	const {roomid} = hummer.createChatRoom();

1. 退出登录
	hummer.logout();
		
1. 更新当前Token
	hummer.refreshToken({});

1. 查询聊天室用户数
	hummer.getChatRoomUserCount({region, roomid});

### chatroom对象

1. 创建聊天室实例Instance
	chatroom = hummer.createChatRoomInstance({region, roomid});

1. 加入聊天室
	chatroom.joinChatRoom()
		
1. 发送单播消息
	chatroom.sendSingleUserMessage({})

1. 发送广播消息
	chatroom.sendGroupMessage({})

1. 发送公屏消息
	chatroom.sendTextChat({})

1. 设置用户信息
	chatroom.setUserAttributes({})

1. 离开聊天室
	chatroom.leaveChatRoom()

1. 接收群组，公屏，对端消息；接收聊天室信息变更，用户属性变更等

| 监听事件                            | 含义                                                         |
| :-------------------------------- | :----------------------------------------------------------- |
| SingleUserMessage                 | 接收单播消息                                                  |
| GroupMessage                      | 接收广播消息                                                  |
| TextChat                          | 接收公屏消息                                                  |
| ChatRoomAttributesUpdated         | 接收聊天室属性变更通知                                         |
| UserKickedOff                     | 用户被踢的广播通知                                             |
| UserCountUpdated                  | 用户数量更新的通知                                             |
| UserOnlineUpdated                 | 进出聊天室的用户列表通知                                        |
| UserAttributesSet                 | 用户属性设置通知                                               |
| UsersMuted                        | 用户被禁言的通知                                               |
| UsersUnMuted                      | 用户解禁的通知                                                 |
| ChatRoomUserOffline               | 当前用户断线超时离开聊天室回调                                   |

		
## 其他接口

1. 查询聊天室属性
	chatroom.getChatRoomAttributes()
		
1. 查询聊天室管理员
	chatroom.getChatRoomManager()
		
1. 查询用户数
	chatroom.getChatRoomUserCount()

1. 获取设置用户属性列表
	chatroom.getUserAttributesList()

1. 查询用户列表
	chatroom.getUserList()
		