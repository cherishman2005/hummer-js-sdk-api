https://github.com/cherishman2005/hummer-js-sdk-api

# 修改点

## [2019-12-26]hummer-chatroom js-sdk1.0.6更新发布
	接口不变，增加部分可靠单播和可靠组播功能
	以下功能及通知修改为可靠信令通道：
	•聊天室单播
	•聊天室广播
	•聊天室成员变更通知
	•聊天室成员人数变更通知

    
## [2019-12-19]hummer-chatroom-1.0.5.js 更新发布
	接口不变，备份非可靠组播版本
    
## [2019-11-19]hummer-chatroom-1.0.4.js 更新发布
	优化接口，达到提供商用标准

## [2019-11-13]hummer-chatroom-1.0.4.js 更新发布
	（1）增加用户属性变更功能

## [2019-09-30]hummer-chatroom-1.0.4.js 更新发布
	（1）将chatroom的connect/login回调调整到hummer（core）模块来处理。
	（2）函数名命名调整：函数名第一个字母从以前的大写字母调整为小些字母

## [2019-09-02]h5service-hummer-sdk-1.0.3.js 更新发布
	uid升级为 64bit整型对应的字符串；

## [2019-08-25]h5service-hummer-sdk-1.0.2.js 更新发布

### 调整 3：
refreshToken老接口没有用json： hummer.refreshToken(uid, token)
调整为json，使用更加方便：
```javascript
hummer.refreshToken({uid, token})
```

# 动态
[2019-08-25]h5service-hummer-sdk-1.0.2.js 更新发布
[2019-08-22] h5service-hummer-sdk-1.0.1.js 正式发布

# js-sdk引用方式

npm路径:
https://www.npmjs.com/package/hummer-chatroom

（1）npm包引用
	npm install hummer-chatroom

	import Hummer from 'hummer-chatroom'

（2）采用cdn(http)方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src="https://***.***.com/hummer-chatroom-x.x.x.js"></script>
```

## chatroom-demo(VUE框架)
  demo功能： 一对一聊天，一对多聊天；

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