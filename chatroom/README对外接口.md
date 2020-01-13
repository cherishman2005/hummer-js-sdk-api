[TOC]
# Hummer chatroom js-sdk

## js-sdk对外接口
### 初始化Hummer
```javascript
const hummer = new Hummer.Hummer({ appid: [appid], 
                            uid: [uid],
                            token: [token],
                            area: [area],  // 'CN'表示中国区，暂支持国内
                            onConnectStatus: onConnectStatus,
                            onLoginStatus: onLoginStatus,
                            onerror: onerror
                          });
```
入参:

| Name                  | Type              |  Description |
| ----------------------| --------------- | -------------- |
|    appid             |      number          | |
|    uid               |      string          | 64bit整型对应的字符串 |
|    token             |      string          ||
|    area              |      string          |'CN'表示中国区，其他表示海外 |
| onConnectStatus      |      Function        | 接收连接状态变更信息  |
| onLoginStatus        |      Function        | 接收登录状态变更消息  |
|    onerror           |      Function        | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"} |

#### createChatRoomId创建聊天室RoomId
请求参数：

| Name                  | Type              |
| ---------------------  | ----------------- |
|    props             |      {[k: string]: string}            |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid             |      number          ||
|    rescode            |      number          | 0：表示成功|
|    props             |      {[k: string]: string}          | | |


示例：
```javascript
let props = {
  "Name": "Hummer聊天室",
  "Description": "测试",
  "Bulletin": "公告",
  "Extention": "自定义",
};

let params = { props };
hummer.createChatRoomId(params).then((res) => {
  console.log("createChatRoomId Res: ", res);
}).catch((err) => {
  console.log(err)
});
```
响应：
```javascript
{appid: 1504984159, uid: "123", roomid: 1704984183, rescode: 0, props: {…}, …}
```

### hummer刷新Token
请求参数：

| Name                  | Type              | Description |
| --------------------- | ----------------- | -------------- |
|    uid             |      string          |  64bit整型对应的字符串 |
|    token             |      string          | |

```javascript
hummer.refreshToken({uid, token}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
});
```

### 初始化ChatRoom
```javascript
const chatroom = new Hummer.ChatRoom(hummer, {  
                                      roomid: roomid,
                                      onRecvSingleUserData: onRecvSingleUserData,
                                      onDismissChatRoomBc: onDismissChatRoomBc,
                                      onUpdateChatRoomInfoBc: onUpdateChatRoomInfoBc,
                                      onKickOffUserBc: onKickOffUserBc,
                                      onRecvGroupBc: onRecvGroupBc,
                                      onTextChatBc: onTextChatBc,
                                      onUserCountBc: onUserCountBc,
                                      onUserOnlineChangeBc: onUserOnlineChangeBc,
                                      onNotifyUserAttributesSet: onNotifyUserAttributesSet,
                                      onerror: onerror
                                      });
```

入参：

| name                     | type | description       |
| -------------------------| ---------- | ----------------- |
|roomid      | number |     房间号         |
|onRecvSingleUserData      | function |      接收对端消息        |
| onDismissChatRoomBc      | function |  接收删除聊天室消息 |
| onUpdateChatRoomInfoBc   | function | 接收更新聊天室消息  |
| onKickOffUserBc          | function |  接收踢人消息 |
| onRecvGroupBc            | function | 接收群组消息  |
| onTextChatBc             | function |  接收公屏消息 |
| onUserCountBc            | function |  接收用户数更新消息 |
| onUserOnlineChangeBc     | function | 接收在线用户变更消息  |
|    onerror               | Function | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"}   |

### 设置接收消息的回调
设置setChatroomRecvCb回调
```javascript
chatroom.setChatroomRecvCb({onRecvSingleUserData: onRecvSingleUserData})
```

### 发送请求消息的接口

#### getInstance获取实例
请求参数：

| Name                  | Type              |
| ---------------------  | ----------------- |
|    NA           |                | |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    appid             |      number          |   |
|    uid               |      string          |   UID |
|    roomid             |      number          | 房间ID |
|    instanceId         |      number          |    实例ID   |

示例：
```javascript
        chatroom.getInstance().then(res => {
          console.log("GetInstance: " + JSON.stringify(res));
        }).catch(err => {
          console.log(err);
        });
```
响应：
```javascript
{"uid":"3800365995","instanceId":2342726287,"isAnonymous":false,"appid":1350626568,"roomid":1550626580}
```

#### joinChatRoom加入聊天室
请求参数：

| Name                  | Type              |
| ---------------------  | ----------------- |
|    joinProps           |      {[k: string]: string}          |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid             |      number          ||
|    rescode            |      number          | 0：表示成功|
|    props             |      {[k: string]: string}          | | |


示例：
```javascript
        let params = { joinProps }
        chatroom.joinChatRoom(params).then((res) => {
          console.log("JoinChatRoom Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```
响应：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"rescode":0,"props":{}}
```

#### leaveChatRoom离开聊天室
请求参数：

| Name                  | Type              |
| --------------------- | ----------------- |
|    NA                 |                ||


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------- |
|    appid             |      number          | |
|    uid             |      string          ||
|    roomid             |      number          ||
|    rescode            |      number          | 0：表示成功|
|    props             |      {[k: string]: string}          | | |


示例：
```javascript
        chatroom.leaveChatRoom().then((res) => {
          console.log("LeaveChatRoom Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })

```
响应：
```javascript
 {"appid":1350626568,"uid":"3800365995","roomid":1550626580,"rescode":0,"props":{}}
```

#### sendGroupBc客户端发送群组消息
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    content                    |     string           | utf-8 |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ---------- |
|    appid             |      number          | |
|    uid             |      string          ||
|    roomid             |      number          ||
|    rescode            |      number          | 0：表示成功|
|    props             |      {[k: string]: string}          | | |


示例：
```javascript
        let params = { content }
        chatroom.sendGroupBc(params).then((res) => {
          console.log("SendGroupBc Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```
响应：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"rescode":0,"props":{}}
```

#### sendSingleUserData客户端给B用户发消息
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    content                    |     string           | utf-8 |
|    receiver                    |     string           | 接收者uid |

响应数据：

| Name                  | Type              |  Description |
| --------------------- |   --------------  |  ----------  |
|    appid             |      number          | |
|    uid             |      string          ||
|    roomid             |      number          ||
|    rescode            |      number          | 0：表示成功|
|    props             |      {[k: string]: string}          | | |


示例：
```javascript
        let params = { content, receiver }
        chatroom.sendSingleUserData(params).then((res) => {
          console.log("SendSingleUserData Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```
响应：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"res":0,"props":{}}
```

#### sendTextChat客户端发送公屏
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    chat                    |     string           | utf-8 |
|    chatProps               |     {[k: string]: string}           |  |
|    extProps                |     {[k: string]: string}           |  |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid            |      number          ||
|    rescode           |      number          | 0：表示成功|
|    props             |      {[k: string]: string}        | | |


示例：
```javascript
        let params = { chat, chatProps, extProps }
        chatroom.sendTextChat(params).then((res) => {
          console.log("SendTextChat Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```
响应：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"rescode":0,"props":{}}
```

#### getChatRoomInfo获取聊天室信息
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    NA                 |                |  | |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    appid             |      number          | |
|    roomid            |      number          ||
|    rescode           |      number          | 0：表示成功|
|    props             |      {[k: string]: string}      |  聊天室介绍信息|


示例：
```javascript
        chatroom.getChatRoomInfo().then((res) => {
          console.log("GetChatRoomInfo Res: " + JSON.stringify(res.toJsonObj()));
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1350626568,"roomid":0,"rescode":0,"props":{"Bulletin":"bull","Owner":"3800365995","MaxUser":"100000","Roomid":"1550626580","Name":"阿武","Passwd":"","AppExtra":"","Appid":"1350626568","Description":"js_sdk测试"}}
```

#### getChatRoomManager获取聊天室所有管理员
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------------- |
|    roler              |    string         | "owner" |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    appid              |      number          | |
|    roomid             |      number          ||
|    admins             |      {[k: string]: Array<string>}        | |
|    rescode            |      number          | 0：表示成功|


示例：
```javascript
        let params = { roler }
        chatroom.getChatRoomManager(params).then((res) => {
          console.log("GetChatRoomManager Res: " + JSON.stringify(res.toJsonObj()));
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1350626568,"roomid":1550626580,"admins":{"owner":["3800365995"]},"rescode":0}
```

#### getUserCount聊天室在线人数
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|          NA           |                |  ||


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    roomid             |      number          ||
|    totalCount             |      number        | |
|    rescode             |      number          | 0：表示成功|

示例：
```javascript
        chatroom.getUserCount().then((res) => {
          console.log("GetUserCount Res: " + JSON.stringify(res));
          return res
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
 {"appid":1350626568,"roomid":1550626580,"totalCount":2,"rescode":0}
```

#### getUserList获取聊天室用户列表
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    num             |      number        |     |
|    pos             |      number        |     |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    roomid            |      number        |     |
|    pos             |      number          | |
|    users             |      Array(string)          | 用户列表 |
|    rescode             |      number          | 0：表示成功|

示例：
```javascript
        let params = { num, pos }
        chatroom.getUserList(params).then((res) => {
          console.log("GetUserList Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1350626568,"roomid":1550626580,"pos":0,"users":["123456","3800365995"],"rescode":0}
```

#### setUserAttributes设置用户属性
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    attributes         |      {[k: string]: string}        |   |  |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid            |      number          ||
|    rescode           |      number          | 0：表示成功|
|    props             |      {[k: string]: string}        | | |


示例：
```javascript
        let attributes = { "Name": "awu", "Description": "js_sdk测试", "Bulletin": "bull", "Extention": "ex" };

        let params = { attributes };
        chatroom.setUserAttributes(params).then((res) => {
          console.log("setUserAttributes Res: ", res);
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{appid: 1350626568, uid: "123", roomid: 1550627349, rescode: 0, props: {…}, …}
```

#### getUserAttributesList获取用户属性列表
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    NA         |      NA        |   |  |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid            |      number          ||
|    rescode           |      number          | 0：表示成功|
|    props             |      {[k: string]: string}        | | |


示例：
```javascript
        chatroom.getUserAttributesList().then((res) => {
          console.log("getUserAttributesList Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1350626568,"roomid":1550627349,"users":{"123":{"Bulletin":"bull","Description":"js_sdk测试","Extention":"ex","Name":"阿武"}},"rescode":0}
```



### 接收消息的接口
| 接收消息的Function         | description       |
| -------------------------- | ----------------- |
|onRecvSingleUserData   |      接收对端消息        |
| onDismissChatRoomBc        |  接收删除聊天室消息 |
| onUpdateChatRoomInfoBc       | 接收更新聊天室消息  |
| onKickOffUserBc      |  接收踢人消息 |
| onRecvGroupBc  | 接收群组消息  |
| onTextChatBc     |  接收公屏消息 |
| onUserCountBc             |  接收用户数更新消息 |
| onUserOnlineChangeBc | 接收在线用户变更消息  |
| onNotifyUserAttributesSet | 接收用户属性设置通知  |
| onConnectStatus       | 接收连接状态变更信息  |
| onLoginStatus         | 接收登录状态变更消息  |

#### onRecvSingleUserData接收对端消息
| Name                  | Type              |  Description |
| --------------------- | --------------- | ----------- |
|    appid              |      number          | |
|    uid                |      string          | 发送者uid|
|    roomid             |      number          ||
|    content             |      string          | 消息内容|

示例：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"content":"哈哈","receiver":"3800365995"}
```

#### onDismissChatRoomBc接收删除聊天室消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    uid             |      string          | 发送者uid|
|    roomid             |      number          ||

示例：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":19181254}
```

#### onUpdateChatRoomInfoBc接收更新聊天室消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    appid              |      number          | |
|    uid                |      string          | 发送者uid|
|    roomid             |      number          ||
|    props              |      {[k: string]: string} |  聊天室介绍信息|

示例：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"props":{}}
```

#### onKickOffUserBc接收踢人消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    appid             |      number          | |
|    roomid             |      number          ||
|    admin             |      string          | 发送者uid|
|    tuids             |      array(string)          | 被踢者UID列表|
|    secs             |      number          |  踢人code|
|    reason             |      string           | 原因|

示例：
```javascript
{"type":"KickOffUserBc","code":0,"appid":1350626568,"roomid":1550626960,"admin":"1234567891234","tuids":["135666911222"],"secs":3000,"reason":"js test KickOffUser","kicktype":0}
```
#### onSendGroupBc接收群组消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    uid             |      string          |发送者uid|
|    roomid             |      number          | |
|    content             |      string          | 消息内容|

示例：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"content":"发送群组消息"}
```

#### onTextChatBc接收公屏消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    uid             |      string          |发送者uid|
|    roomid             |      number          ||
|    chat                    |     string           | utf-8 |
|    chatProps                   |     {[k: string]: string}          |  |
|    extProps                    |     {[k: string]: string}           |  |

示例：
```javascript
{"appid":1350626568,"uid":"3800365995","roomid":1550626580,"chat":"你好","chatProps":{},"extProps":{}}
```

#### onUserCountBc接收用户数更新消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    roomid             |      number          ||
|    totalCount             |      number        | |

示例：
```javascript
{"appid":1350626568,"roomid":1550626580,"totalCount":2}
```

#### onUserOnlineChangeBc接收在线用户变更消息
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    roomid             |      number          ||
|    joinUsers             |      array(string)          | 加入用户列表|
|    leaveUsers             |      array(string)          | 离开用户列表|

示例：
```javascript
{"appid":1350626568,"roomid":1550626580,"joinUsers":[],"leaveUsers":["123456"]}
```

#### onNotifyUserAttributesSet接收用户属性设置通知
| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    uid               |      string          ||
|    roomid             |      number          ||
|    attributes         |      {[k: string]: string}        |   |  |

示例：
```javascript
{"appid":1350626568,"uid":"123","roomid":1550627349,"attributes":{"Bulletin":"bull","Description":"js_sdk测试","Extention":"ex","Name":"阿武"}}
```


## 调试示例demo

https://service-test.sunclouds.com/room/chat

### chatroom-demo配置

采用http(CDN)方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src=" https://***.**.com/hummer-chatroom-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。

【注】js_sdk带上版本号发布（如“1.0.2”）。

nginx.conf
```javascript
	location /room {
		try_files $uri $uri/ /room/index.html;
		default_type text/html;
		alias /home/zhangbiwu/vue_projects/room/dist;
	}
```
