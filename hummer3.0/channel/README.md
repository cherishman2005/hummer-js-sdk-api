[TOC]

# Hummer Channel js-sdk

https://github.com/cherishman2005/hummer-js-sdk-api/

## FAQ

### 动态

[2019-04-03] 【hummer-channel-sdk新版本】提供hummer-channel-sdk 2.1.1版本

接口调整，详见接口hummer3.0文档：《hummer3.0-channel对外接口.md》

主要新增功能如下：

用户属性管理主要新增API：

	addOrUpdateUserAttributes	添加或更新本地用户的属性
	clearUserAttributes	清空本地用户的属性

频道属性管理主要新增API：

	setChannelAttributes 	全量设置某指定频道的属性
	addOrUpdateChannelAttributes	更新频道属性
	deleteChannelAttributesByKeys	删除某指定频道的指定属性
	clearChannelAttributes	清空某指定频道的属性
	getChannelAttributes	查询某指定频道的全部属性
	getChannelAttributesByKeys	查询某指定频道指定属性名的属性


[2019-03-25] 【hummer-channel-sdk新版本】提供hummer-channel-sdk 2.1.0版本

接口调整，详见接口hummer3.0文档：《hummer3.0-channel对外接口.md》

主要新增功能如下：

	1.HA多队列功能；（sdk内部优化）；
	2.token功能；


[2019-03-03] 【hummer-channel-sdk新版本】提供hummer-channel-sdk.js  2.0.0版本

【重点】在hummer-channel.js  1.0.5版本的基础上做了接口调整。新业务请使用“hummer-channel-sdk”版本。

接口调整，详见接口hummer3.0文档：《hummer3.0-channel对外接口.md》

主要新增功能如下：

	1.全球化功能
	2.【批量查询】查询单个或多个频道的成员人数
	3.【批量查询】查询指定用户的在线状态。
	4.登录/登出API接口
	5.试 js-sdk端与移动端sdk的消息互通


## hummer-channel-sdk js-sdk安装配置

npm包路径：https://www.npmjs.com/package/hummer-channel-sdk

（1）npm包

npm install hummer-channel-sdk

```javascript
import Hummer from 'humer-channel-sdk'
```

（2）采用http方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src="./path/hummer-channel-sdk-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。


## 线上demo

https://hummer-test.sunclouds.com/room-channel-test/channel-test

## 调试示例demo

https://github.com/cherishman2005/vue-room

### channel-demo配置

nginx.conf
```javascript
	location /room {
		try_files $uri $uri/ /room/index.html;
		default_type text/html;
		alias /home/zhangbiwu/vue_projects/room/dist;
	}
```
