
# hummer-channel接入流程

## hummer-channel主要接入流程

	1 Hummer初始化
		hummer = Hummer.createHummer({});;
		需要的入参包括：appid, uid, token等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852

	2 ChannelService初始化
		client = hummer.createInstance();


	3 P2P的消息处理

		3.1 设置用户归属地
			client.setUserRegion({ region });
		
		3.2 发送P2P的消息
			client.sendMessageToUser({});

		3.3 接收对端消息
			client.on('MessageFromUser', (data) => { console.log(data); });
		
		3.4 查询单个用户在线
			client.queryOnlineStatusForUser({uid: uid});
		
		3.5 批量查询用户在线
			client.queryUsersOnlineStatus({ uids: uids })


	4 频道消息处理
	
		4.1 先要创建一个频道实例；
			channel = client.createChannel({ region, channelId })
		
		4.2 加入频道；
			channel.joinChannel()
		
		4.3 给指定channel发送组播消息
			channel.sendMessageToChannel({})

		4.4 设置用户属性(setUserAttributes)
			channel.setUserAttributes({})
		
		4.5 删除用户某些属性(deleteUserAttributesByKeys)
			channel.deleteUserAttributesByKeys({})

		4.6 获取频道用户列表(getChannelUserList)
			channel.getChannelUserList({})
		
		4.7 根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)
			channel.getChannelUserListByAtrribute({})
		
		4.8 查询单个或多个频道用户数(getChannelUserCount)
			channel.getChannelUserCount({ channelIds: channelIds });
		
		4.9 接收组播消息
			channel.on('ChannelMessage', (data) => { console.log(data); });
		
		4.10 接收加入频道通知
			channel.on('NotifyJoinChannel', (data) => { console.log(data); });
		
		4.11 接收离开频道通知
			channel.on('NotifyLeaveChannel', (data) => { console.log(data); });
		
		4.12 接收用户属性设置通知
			channel.on('NotifyUserAttributesSet', (data) => { console.log(data); });
		
		4.13 接收用户属性删除通知
			channel.on('NotifyUserAttributesDelete', (data) => { console.log(data); });
		
		4.14 接收用户数变更通知
			channel.on('NotifyUserCountChange', (data) => { console.log(data); });
		
		4.15 退出channel
			channel.leaveChannel()
		


## 其他接口

	（1） 查询Instance实例信息
		hummer.getInstanceInfo();

