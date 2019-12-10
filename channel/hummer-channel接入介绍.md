
# hummer-channel接入流程

## hummer-channel主要接入流程

	（1）Hummer初始化
		hummer = new Hummer.Hummer({});
		需要的入参包括：appid, uid, token等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852

	（2）ChannelService初始化
		channel = new Hummer.ChannelService({hummer, channel的回调obj})

	（3）加入channel
		channel.joinChannel(channelId)
		
	（4）发送P2P的消息
		channel.sendMessageToUser({})

	（5）给指定channel发送组播消息
		channel.sendMessageToChannel({})

	（6）设置用户属性(setUserAttributes)
		channel.setUserAttributes({})
		
	（7）删除用户某些属性(deleteUserAttributesByKeys)
		channel.deleteUserAttributesByKeys({})

	（8）获取频道用户列表(getChannelUserList)
		channel.getChannelUserList({})
		
	（9）根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)
		channel.getChannelUserListByAtrribute({})
		
	（10）接收频道内用户在线更新，组播消息，对端消息，用户属性变更消息等
		在初始化channel时通过入参设置回调；
		
	（11）离开channel
		channel.leaveChannel()

## 其他接口

	（1） 查询Instance实例
		channel.getInstance();
	
	（2）token刷新接口
		hummer.refreshToken({uid, token});

