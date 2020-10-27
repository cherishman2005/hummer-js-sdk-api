# Hummer chatroom js-sdk

## 动态

[2020-10-27] hummer-chatroom-sdk 2.1.6

1. Token过期&更新机制
   * 新增刷新token的API：refreshToken1；废弃API: refreshToken；
   * 新增监听token即将过期接口: hummer.on("TokenWillExpire", () => {});

1. 多端在线与登录互踢机制

1. 获取连接所处状态
   * 新增API：getConnectionState；废弃API：getState

1. 连接状态变化回调优化

1. sdk内部优化

[2020-08-25] hummer-chatroom-sdk 2.1.5

对外接口不变。sdk内部优化：新增HA功能

[2020-07-30] hummer-chatroom-sdk 2.1.4

新增js-sdk支持多端登录互踢功能

[2020-07-20] hummer-chatroom-sdk 2.1.3
【重要】域名从web-ap-service.sunclouds.com更新为web-ap-service.jocloud.com

项目需要web-ap-service.sunclouds.com停用。必须更新新的js-sdk，新域名为web-ap-service.jocloud.com

[2020-07-17] hummer-chatroom-sdk 2.1.2

[2020-06-11] hummer-chatroom-sdk 2.1.1

[2020-06-02] hummer-chatroom-sdk 2.1.0

chatroom js-sdk对外发布首个版本 2.1.0

[2020-05-13] hummer-chatroom-sdk 2.0.3版本

[2020-04-23] hummer-chatroom-sdk 2.0.2版本

hummer-chatroom-sdk hummer3.x-beta版本

chatroom js-sdk npm包下载地址：

https://www.npmjs.com/package/hummer-chatroom-sdk


## hummer-chatroom-sdk js-sdk安装配置

npm包路径：https://www.npmjs.com/package/hummer-chatroom-sdk

（1）npm包

npm install hummer-chatroom-sdk

```javascript
import Hummer from 'hummer-chatroom-sdk'
```

（2）采用http方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src="./path/hummer-chatroom-sdk-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。
