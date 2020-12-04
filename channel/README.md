# Hummer Channel Service js-sdk

## 动态

[2020-10-20] 提供hummer-channel.js 1.0.8 版本
	接口未改动。sdk内部优化。

【注】
* 1.0.6和1.0.7只是更新域名和兼容屏蔽https上报
* 1.0.8版本在2019-12-11 hummer-channel.js 1.0.5基础上合入hummer-rts js-sdk的迭代更新和优化


[2020-10-20] 提供hummer-channel.js 1.0.7 版本

	接口未改动。sdk内部优化：对Electron兼容适配。

[2020-07-20] hummer-channel 1.0.6

【重要】域名从web-ap-service.sunclouds.com更新为web-ap-service.jocloud.com

项目需要停用web-ap-service.sunclouds.com。必须更新新的js-sdk，新域名为web-ap-service.jocloud.com

[2019-12-11] 提供hummer-channel.js 1.0.5版本

	接口未改动：（I）js-sdk内部心跳超时优化；（II）js-sdk内部可靠组播优化；

[2019-12-02] 提供hummer-channel.js 1.0.4版本

	接口未改动，js-sdk内部优化

[2019-11-29] 提供hummer-channel-1.0.3.js版本

调整接口onNotifyJoinChannel和onNotifyLeaveChannel接口

进出频道回调改为聚合多个用户通知：


（1）onNotifyJoinChannel({channelId: string, uid: string})

改为

	onNotifyJoinChannel({channelId: string, uids: string[]})


（2）onNotifyLeaveChannel({channelId: string, uid: string})

改为

	onNotifyLeaveChannel({channelId: string, uids: string[]})


[2019-11-19] 提供hummer-channel-1.0.2.js版本

	接口未改动，内部优化

[2019-11-13] 提供hummer-channel-1.0.1.js版本

	接口未改动，串行操作内部优化

[2019-11-05] 提供hummer-channel-1.0.1.js版本

	接口未改动，js-sdk内部优化


[2019-10-30] 提供hummer-channel-1.0.0.js版本

主要支持功能：

	（1）P2P 可靠，非可靠消息；
	（2）组播 可靠，非可靠消息；
	（3）频道用户上下行通知；
	（4）用户属性变更通知；
	（5）单个用户查询在线状态；
	（6）与SignalService兼容互通；

## hummer-channel js-sdk安装配置

（1）npm包

 npm install hummer-channel
 
 ```javascript
 import Hummer from 'hummer-channel'
 ```
 
（2）采用http(CDN)方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src=" https://***.**.com/hummer-channel-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。


## hummer-channel API接口文档

- [https://github.com/cherishman2005/hummer-js-sdk-api/](https://github.com/cherishman2005/hummer-js-sdk-api/)
