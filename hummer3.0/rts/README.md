[TOC]

# Hummer RTS js-sdk


## 动态

[2020-07-07] hummer-rts-sdk 3.2.0

# 3.2.x新版本与3.1.x旧版本兼容：

用户属性与频道属性操作8个API做了是否发送广播的控制开关优化

| API       | 功能   | 
| ---------- | ------ |
| setUserAttributes      | room设置本地用户属性 | 
| deleteUserAttributesByKeys     | room删除本地用户某些属性 |
| addOrUpdateUserAttributes     | room添加或更新本地用户的属性 | 
| clearUserAttributes           | room清空本地用户的属性 | 
| setRoomAttributes      | room全量设置某指定房间的属性 | 
| deleteRoomAttributesByKeys     | room删除某指定房间的指定属性 |
| addOrUpdateRoomAttributes     | room更新房间属性 | 
| clearRoomAttributes           | room清空某指定房间的属性 | 

* `3.1.x 旧版本SDK无对应字段，后台默认发送广播`

* `3.2.x 新版本SDK默认不发广播，由业务控制是否需要发送广播`


## hummer-rts-sdk js-sdk安装配置

npm包路径：https://www.npmjs.com/package/hummer-rts-sdk

（1）npm包

最新版本引入:
```javascript
npm install hummer-rts-sdk
```

指定版本引入：
```javascript
npm install hummer-rts-sdk@x.x.x
```


```javascript
import Hummer from 'humer-rts-sdk'
```

（2）采用http方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src="./path/hummer-rts-sdk-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。
