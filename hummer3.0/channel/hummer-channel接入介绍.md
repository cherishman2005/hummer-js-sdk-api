
# hummer-channel接入流程

## hummer-channel主要接入流程

	1 Hummer初始化
		hummer = Hummer.createHummer({});;
		需要的入参包括：appid, uid, token等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852


	2 Hummer登录初始化
		hummer.login();

	3 设置用户归属地
		hummer.setUserRegion({ region });

	4 ChannelService初始化
		client = hummer.createInstance();

	5 P2P的消息处理
		5.1 发送P2P的消息
			client.sendMessageToUser({});

		5.2 接收对端消息
			client.on('MessageFromUser', (data) => { console.log(data); });
		
		5.3 批量查询用户在线
			client.queryUsersOnlineStatus({ uids: uids })


	6 频道消息处理
	
		6.1 先要创建一个频道实例；
			channel = client.createChannel({ region, channelId })
		
		6.2 加入频道；
			channel.joinChannel()
		
		6.3 给指定channel发送组播消息
			channel.sendMessageToChannel({})

		6.4 设置用户属性(setUserAttributes)
			channel.setUserAttributes({})
		
		6.5 删除用户某些属性(deleteUserAttributesByKeys)
			channel.deleteUserAttributesByKeys({})

		6.6 获取频道用户列表(getChannelUserList)
			channel.getChannelUserList({})
		
		6.7 获取指定用户指定属性名的属性(getUserAttributesByKeys)
			channel.getUserAttributesByKeys({})

		6.8 获取指定用户的全部属性(getUserAttributes)
			channel.getUserAttributes({})
		
		6.9 查询单个或多个频道用户数(getChannelUserCount)
			channel.getChannelUserCount({ channelIds: channelIds });
		
		6.10 接收组播消息
			channel.on('ChannelMessage', (data) => { console.log(data); });
		
		6.11 接收加入频道通知
			channel.on('NotifyJoinChannel', (data) => { console.log(data); });
		
		6.12 接收离开频道通知
			channel.on('NotifyLeaveChannel', (data) => { console.log(data); });
		
		6.13 接收用户属性设置通知
			channel.on('NotifyUserAttributesSet', (data) => { console.log(data); });
		
		6.14 接收用户属性删除通知
			channel.on('NotifyUserAttributesDelete', (data) => { console.log(data); });
		
		6.15 接收用户数变更通知
			channel.on('NotifyUserCountChange', (data) => { console.log(data); });
		
		6.16 退出channel
			channel.leaveChannel()
		


## 其他接口
	（1） 退出登录
		hummer.logout();
		
	（2） 更新当前Token
		hummer.refreshToken({});
	
	（3） 查询Instance实例信息
		hummer.getInstanceInfo();
