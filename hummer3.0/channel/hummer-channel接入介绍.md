
# hummer-Room接入流程

## hummer-Room主要接入流程

	1 Hummer初始化
		hummer = Hummer.createHummer({appid});
		需要的入参包括：appid等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852


	2 登录
		hummer.login({region, uid, token});

	3 RoomService初始化
		client = hummer.createClient();

	4 P2P的消息处理
		4.1 发送P2P的消息
			client.sendMessageToUser({});

		4.2 接收对端消息
			client.on('MessageFromUser', (data) => { console.log(data); });
		
		4.3 批量查询用户在线
			client.queryUsersOnlineStatus({ uids: uids })


	5 频道消息处理
	
		5.1 先要创建一个频道实例；
			room = client.createRoom({ region, RoomId })
		
		5.2 加入频道；
			room.joinRoom()
		
		5.3 给指定Room发送组播消息
			room.sendMessageToRoom({})

		5.4 设置用户属性(setUserAttributes)
			room.setUserAttributes({})
		
		5.5 删除用户某些属性(deleteUserAttributesByKeys)
			room.deleteUserAttributesByKeys({})

		5.6 获取频道用户列表(getRoomUserList)
			room.getRoomUserList({})
		
		5.7 获取指定用户指定属性名的属性(getUserAttributesByKeys)
			room.getUserAttributesByKeys({})

		5.8 获取指定用户的全部属性(getUserAttributes)
			room.getUserAttributes({})
		
		5.9 查询单个或多个频道用户数(getRoomUserCount)
			client.getRoomUserCount({ region, roomIds });
		
		5.10 接收组播消息
			room.on('RoomMessage', (data) => { console.log(data); });
		
		5.11 接收加入频道通知
			room.on('NotifyJoinRoom', (data) => { console.log(data); });
		
		5.12 接收离开频道通知
			room.on('NotifyLeaveRoom', (data) => { console.log(data); });
		
		5.13 接收用户属性设置通知
			room.on('NotifyUserAttributesSet', (data) => { console.log(data); });
		
		5.14 接收用户属性删除通知
			room.on('NotifyUserAttributesDelete', (data) => { console.log(data); });
		
		5.15 接收用户数变更通知
			room.on('NotifyUserCountChange', (data) => { console.log(data); });
		
		5.16 退出Room
			room.leaveRoom()
		


## 其他接口
	（1） 退出登录
		hummer.logout();
		
	（2） 更新当前Token
		hummer.refreshToken({});
	
	（3） 查询Instance实例信息
		hummer.getInstanceInfo();
