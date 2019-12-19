
# hummer-chatroom接入流程

## 主要接入流程

	（1）Hummer初始化
		hummer = new Hummer.Hummer({});
		需要的入参包括：appid, uid, token等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852

	（2）创建聊天室RoomId
		hummer.createChatRoomId({props})
		【注】需要新建聊天室时才调用createChatRoomId生成roomid；

	（3）ChatRoom聊天室初始化
		chatroom = new Hummer.ChatRoom(hummer, {roomid, chatroom的回调obj})
		
	（4）加入聊天室
		chatroom.joinChatroom()
		
	（5）A给B用户发送消息
		chatroom.sendSingleUserData({})

	（6）发送群组消息
		chatroom.sendGroupBc({})

	（7）发送公屏消息
		chatroom.sendTextChat({})
		
	（8）设置用户信息
		chatroom.setUserAttributes({})

	（9）接收群组，公屏，对端消息；接收聊天室信息变更，用户属性变更等
		在初始化chatroom时通过入参设置回调；
		或者在chatroom初始化后通过chatroom.setChatroomRecvCb({})设置
		
	（10）离开聊天室
		chatroom.leaveChatRoom()

		
## 其他接口

	（1）token刷新接口
		hummer.refreshToken({uid, token})
		
	（2） 查询Instance实例
		chatroom.getInstance()
		
	（3）查询聊天室信息
		chatroom.getChatRoomInfo()
		
	（4）查询聊天室管理员
		chatroom.getChatRoomManager()
		
	（5）查询用户数
		chatroom.getUserCount()

	（6）获取设置用户属性列表
		chatroom.getUserAttributesList()

	（7）查询用户列表
		chatroom.getUserList()
		
