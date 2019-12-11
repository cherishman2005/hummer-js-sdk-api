
# hummer-im接入流程

## IM P2PChat主要接入流程

	（1）Hummer初始化
		hummer = new Hummer.Hummer({});
		需要的入参包括：appid, uid, token等信息；
		
		uid和token生成方法详见：
		https://www.sunclouds.com/cloud/v2/developer/doc.htm?serviceId=102&typeCode=FAQ&title=Python&version=2.0&parentId=852
		
		【注】寻欢IM P2PChat不采用如上token生成方法；采用token_type="YY_TOKEN" YY账号接入新通道。

	（2）IM聊天室初始化
		im = new Hummer.IM({hummer, im的回调obj})

	（3）初始化拉取消息InitPullMsg
		im.initPullMsg({mode});
		
		【注】寻欢第一期没有存储，采用mode=1；（首次以上次关闭时的时间点拉取聊天记录）

	（4）A给B用户发送消息
		im.sendP2PChat({ content, msg_type, receiver, onmessage })

		【注】寻欢第一期只支持msg_type=0（text格式）发送
	
	（5）接收onRecvIMPushMsg消息
		在初始化IM时通过入参设置回调；
		或者在IM初始化后通过im.setIMRecvCb({})设置
		
## 其他接口

	（1） 查询Instance实例
		im.getInstance();
	
	（2）token刷新接口
		hummer.refreshToken({uid, token});

