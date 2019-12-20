[TOC]

# Hummer Signal Service js-sdk

## FAQ

### 动态
[2019-12-11] 提供hummer-signal-1.0.3.js版本

	接口未改动；内部优化升级；

[2019-11-13] 提供hummer-signal-1.0.3.js版本

	接口未改动；对串行操作内部优化；

[2019-11-05] 提供hummer-signal-1.0.3.js版本

	接口未改动；根据channelService的测试，进行同步优化；

[2019-10-12] 提供hummer-signal-1.0.2.js版本

	（1）将signal的connect/login回调调整到hummer（core）模块来处理。详见下面的描述
	（2）reliable调整到option对象；详见下面的描述

[2019-09-29] 提供hummer-signal-1.0.2.js版本

	区分可靠P2P、非可靠P2P；可靠组播、非可靠组播；
    
    【注】非可靠：这里非可靠是在网络异常，或抖动时才会出现掉包；——非可靠可以满足绝大部分场景；
    
    对于高并发，时延要求高的场景建议采用非可靠设置（如白板）； 对于可靠性要求高的场景采用可靠设置；
    
（I）白板功能调试主要场景：（时延较小，满足需求）

    （a）【1对1白板授课】非可靠的p2p；

    （b）【1对多白板授课】非可靠的组播；

（II）另外一个场景是1对多，老师禁掉其他学生的声音的控制；采用可靠p2p
    

[2019-09-16] 提供hummer-signal-1.0.0.js版本

新增完善接口说明:

	（1）新增初始化回调onerror；
	（2）send/recv以二进制形式；——我们提供2个 string <--> utf8二进制的接口供业务使用（业务也可以用自己的）

## js-sdk引用方式

npm路径:
https://www.npmjs.com/package/hummer-signal

（1）npm包引用
	npm install hummer-signal

	import Hummer from 'hummer-signal'

（2）采用cdn(http)方式引用signal js_sdk
```javascript
<script charset="utf-8" src="https://***.***.com/hummer-signal-x.x.x.js"></script>
```

## 调试示例demo

https://github.com/cherishman2005/vue-room

### signal-demo配置

nginx.conf
```javascript
	location /room {
		try_files $uri $uri/ /room/index.html;
		default_type text/html;
		alias /home/zhangbiwu/vue_projects/room/dist;
	}
```