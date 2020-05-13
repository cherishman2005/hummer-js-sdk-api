# Hummer chatroom js-sdk

## 动态

[2019-05-13] 【hummer-chatroom-sdk新版本】hummer-chatroom-sdk 2.0.3版本

[2019-04-23] hummer-chatroom-sdk 2.0.2版本

hummer-chatroom-sdk hummer3.x-beta版本

chatroom js-sdk npm包下载地址：

https://www.npmjs.com/package/hummer-chatroom-sdk


## hummer-chatroom-sdk js-sdk安装配置

npm包路径：https://www.npmjs.com/package/hummer-chatroom-sdk

（1）npm包

npm install hummer-chatroom-sdk

```javascript
import Hummer from 'humer-chatroom-sdk'
```

（2）采用http方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src="./path/hummer-chatroom-sdk-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。


## 线上demo

https://hummer-test.sunclouds.com/room/chat-test

## 调试示例demo

https://github.com/cherishman2005/vue-room

### chatroom-demo配置

nginx.conf
```javascript
	location /room {
		try_files $uri $uri/ /room/index.html;
		default_type text/html;
		alias /home/zhangbiwu/vue_projects/room/dist;
	}
```
