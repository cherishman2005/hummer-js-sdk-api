<!-- TOC -->

- [Hummer chatroom js-sdk](#hummer-chatroom-js-sdk)
    - [js-sdk对外接口](#js-sdk对外接口)
        - [注意事项](#注意事项)
        - [createHummer](#createhummer)
            - [setLogLevel](#setloglevel)
            - [getState](#getstate)
            - [login](#login)
            - [logout](#logout)
            - [接收连接状态变更的回调通知(hummer.on('ConnectionStateChanged', (data) => {}))](#接收连接状态变更的回调通知hummeronconnectionstatechanged-data--)
            - [接收登录互踢的回调通知(hummer.on('ForceoutOffline', (data) => {}))](#接收登录互踢的回调通知hummeronforceoutoffline-data--)
            - [接收Token过期的回调通知(hummer.on('TokenExpired', () => {}))](#接收token过期的回调通知hummerontokenexpired---)
            - [refreshToken](#refreshtoken)
            - [createChatRoom](#createchatroom)
            - [getChatRoomUserCount](#getchatroomusercount)
        - [ChatRoom](#chatroom)
            - [createChatRoomInstance](#createchatroominstance)
            - [joinChatRoom](#joinchatroom)
            - [leaveChatRoom](#leavechatroom)
            - [sendGroupMessage](#sendgroupmessage)
            - [sendSingleUserMessage](#sendsingleusermessage)
            - [sendTextChat](#sendtextchat)
            - [getChatRoomAttributes](#getchatroomattributes)
            - [getChatRoomManager](#getchatroommanager)
            - [getUserList](#getuserlist)
            - [setUserAttributes](#setuserattributes)
            - [getUserAttributesList](#getuserattributeslist)
            - [muteUser](#muteuser)
            - [getMutedUserList](#getmuteduserlist)
        - [接收消息的监听接口](#接收消息的监听接口)
                - [接收单播消息(chatroom.on('SingleUserMessage', (data) => {}))](#接收单播消息chatroomonsingleusermessage-data--)
            - [接收公屏消息(chatroom.on('TextChat', (data) => {}))](#接收公屏消息chatroomontextchat-data--)
            - [接收广播消息(chatroom.on('GroupMessage', (data) => {}))](#接收广播消息chatroomongroupmessage-data--)
            - [接收广播消息(chatroom.on('ChatRoomAttributesUpdated', (data) => {}))](#接收广播消息chatroomonchatroomattributesupdated-data--)
            - [接收用户被踢的广播通知(chatroom.on('UserKickedOff', (data) => {}))](#接收用户被踢的广播通知chatroomonuserkickedoff-data--)
            - [接收用户数更新消息(chatroom.on('UserCountUpdated', (data) => {}))](#接收用户数更新消息chatroomonusercountupdated-data--)
            - [接收在线用户变更消息(chatroom.on('UserOnlineUpdated', (data) => {}))](#接收在线用户变更消息chatroomonuseronlineupdated-data--)
            - [接收用户属性设置通知(chatroom.on('UserAttributesSet', (data) => {}))](#接收用户属性设置通知chatroomonuserattributesset-data--)
            - [接收被禁言的广播通知(chatroom.on('UsersMuted', (data) => {}))](#接收被禁言的广播通知chatroomonusersmuted-data--)
            - [接收解禁的广播通知(chatroom.on('UsersUnMuted', (data) => {}))](#接收解禁的广播通知chatroomonusersunmuted-data--)
            - [接收当前用户断线超时离开聊天室回调(chatroom.on('ChatRoomUserOffline', () => {}))](#接收当前用户断线超时离开聊天室回调chatroomonchatroomuseroffline---)

<!-- /TOC -->

# Hummer chatroom js-sdk

## js-sdk对外接口

### 注意事项

（1）本系统支持使用64位数字的用户ID(uid)。由于javascript的number类型不支持64位的整数。所以sdk使用string来存储uid。

### createHummer

Hummer初始化：创建hummer实例

```javascript
    hummer = Hummer.createHummer({ appid });
```

入参:

| Name       | Type   | Description                                                                              |
| ---------- | ------ | ---------------------------------------------------------------------------------------- |
| appid      | number |  项目的appid                                                                             |

传入项目的appid，32bit number 类型。

#### setLogLevel

设置打印日志级别

```javascript
hummer.setLogLevel(level);
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
|   level   |     enum                |    日志等级                                         |

| 枚举值 | 含义 |
| :--- | :--- |
| DEBUG(-1) | debug |
| LOG(0) | log |
| INFO(1) | info |
| WARN(2) | warn |
| ERROR(3) | error |

响应数据：（无）

```typescript
enum LOGLEVEL {
	DEBUG = -1,
	LOG = 0,
	INFO = 1,
	WARN = 2,
	ERROR = 3
}
```

#### getState

获取SDK当前状态

```javascript
hummer.getState();
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| NA        |                         |                                                     |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|           | enum   |                             |

| 枚举值 | 含义 |
| :--- | :--- |
| DISCONNECTED | 未连接 |
| CONNECTING | 连接中 |
| RECONNECTING | 重连中 |
| CONNECTED | 已连接 |


#### login

登录

```javascript
hummer.login({ region, uid, token });
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region?   | string                  |     用户归属地region （"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"）               |
| uid       | string                  |     用户UID                                         |
| token?    | string                  |     可选的动态密钥                                  |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

【注】
* token支持3种模式：appid模式、token模式、临时token模式
* 在appid模式下，不用填token；其他模式下必须填token

#### logout

登出

```javascript
hummer.logout();
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
|   NA      |                         |                 |                                   |


响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


#### 接收连接状态变更的回调通知(hummer.on('ConnectionStateChanged', (data) => {}))

| 事件            | 描述                 |
| --------------- | --------------------------- |
| ConnectionStateChanged | 通知 SDK 与 Hummer RTS 系统的连接状态发生了改变。 |

```javascript
hummer.on('ConnectionStateChanged', (data) => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ConnectionStateChanged" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| state   | enum    | 新的连接状态                |
| reason  | enum    | 状态改变的原因              |

```typescript
enum ConnectionState {
    DISCONNECTED = "DISCONNECTED",
    CONNECTING = "CONNECTING",
    CONNECTED = "CONNECTED",
    RECONNECTING = "RECONNECTING",
};

enum ConnectionChangeReason {
    INIT = 'INIT',
    LOGIN = "LOGIN",
    LOGIN_SUCCESS = "LOGIN_SUCCESS",
    LOGIN_FAILURE = "LOGIN_FAILURE",
    LOGIN_TIMEOUT ="LOGIN_TIMEOUT",
    INTERRUPTED ="INTERRUPTED",
    LOGOUT = "LOGOUT",
    REMOTE_LOGIN = "REMOTE_LOGIN"
};
```

链接初始状态：{"state":"DISCONNECTED","reason":"INIT"}

（1）登录过程中：
回调通知{"state":"CONNECTING","reason":"LOGIN"}；

（2）登录完成：
回调通知{"state":"CONNECTED","reason":"LOGIN_SUCCESS"}

（3）登录失败：
回调通知{"state":"DISCONNECTED","reason":"LOGIN_FAILURE"}

（4）登录超时失败：
回调通知{"state":"DISCONNECTED","reason":"LOGIN_TIMEOUT"}

（5）重连/重登：
回调通知{"state":"RECONNECTING","reason":"INTERRUPTED"}；

（6）账号登录被踢：（js-sdk支持多端登录互踢功能）
回调通知{"state":"DISCONNECTED","reason":"REMOTE_LOGIN"}


#### 接收登录互踢的回调通知(hummer.on('ForceoutOffline', (data) => {}))

| 事件            | 描述                 |
| --------------- | --------------------------- |
| ForceoutOffline | 通知js-sdk账号登录被踢        |

```javascript
hummer.on('ForceoutOffline', (data) => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ForceoutOffline" |
| handler   | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| code   | number    | 下线原因分类码              |
| reason  | string   | 下线原因描述                |


账号登录被踢的3种情形：

* { code: 100, resaon: 您的帐号在别处登录，您被迫下线。}

* { code: 101, resaon: 超出终端类限制，被其他端踢下线。 }

* { code: 102, resaon: 超出设备数限制，被其他端踢下线。  }


#### 接收Token过期的回调通知(hummer.on('TokenExpired', () => {}))

```javascript
hummer.on('TokenExpired', () => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string    | 取值"TokenExpired"       |
| handler   | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| NA      |         |  只用监听事件，data为空       |

【注】
* 断链重连时token过期才会回调通知。

#### refreshToken

更新当前Token

```javascript
hummer.refreshToken({ token });
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| token     | string                  |           动态密钥                                  |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


#### createChatRoom

创建聊天室

```javascript
  hummer.createChatRoom({})
```

请求参数：


| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region    | string                  | chatroom归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| attributes        |      {[k: string]: string}       |   房间属性       |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
| rescode               | number            | 0：表示成功    |
| msg                   | string            | 返回描述       |
| roomid                | number            |  聊天室ID      |


示例：
```javascript
let attributes = {
  "Name": "Hummer聊天室",
  "Description": "测试",
  "Bulletin": "公告",
  "Extention": "自定义",
};

let options = { region, attributes };
hummer.createChatRoom(options).then((res) => {
  console.log("createChatRoom res:", res);
}).catch((err) => {
  console.log(err)
});
```

响应：
```javascript
{appid: 1504984159, uid: "123", roomid: 1704984183, rescode: 0, props: {…}, …}
```

#### getChatRoomUserCount

查询聊天室在线人数

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region   | string                  |     region （"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomid   | number                  |  聊天室ID      |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    rescode            |      number       | 0：表示成功  |
|    totalCount         |      number       | |

示例：
```javascript
  hummer.getChatRoomUserCount({region, roomid}).then((res) => {
    console.log("getChatRoomUserCount Res: " + JSON.stringify(res));
  }).catch(err => {
    console.log(err)
  })
```

响应：
```javascript
{"appid":1504984159,"roomid":1804994216,"totalCount":0,"rescode":0}
```

### ChatRoom

#### createChatRoomInstance

创建ChatRoom聊天室实例

```javascript
	chatroom = hummer.createChatRoomInstance({region, roomid});
```

#### joinChatRoom

加入聊天室

```javascript
	chatroom.joinChatRoom({});
```

请求参数：

| Name                  | Type              |  Description                 |
| --------------------- | ----------------- |  ------------  |
|    joinProps          |  {[k: string]: string} | 扩展字段，用于业务扩展，SDK 只负责透传 |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    rescode            |      number          | 0：表示成功|


示例：
```javascript
        let params = { joinProps }
        chatroom.joinChatRoom(params).then((res) => {
          console.log("JoinChatRoom Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

#### leaveChatRoom

离开聊天室

```javascript
	chatroom.leaveChatRoom();
```

请求参数：（无）

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------  |
| rescode               |      number       | 0：表示成功   |
| msg                   | string            | 返回描述      |

示例：
```javascript
        chatroom.leaveChatRoom().then((res) => {
          console.log("LeaveChatRoom Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------  |
| rescode               |      number       | 0：表示成功   |
| msg                   | string            | 返回描述      |

示例：
```javascript
        chatroom.leaveChatRoom().then((res) => {
          console.log("LeaveChatRoom Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

#### sendGroupMessage

发送群组消息

```javascript
	chatroom.sendGroupMessage({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| content               | string            | 内容            |
| kvExtra               | {[k: string]: string}  | 扩展字段key-value |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ---------- |
| rescode               |      number       | 0：表示成功   |
| msg                   | string            | 返回描述      |

示例：
```javascript
  let params = { content, kvExtra }
  chatroom.sendGroupMessage(params).then((res) => {
    console.log("sendGroupMessage Res: " + JSON.stringify(res));
  }).catch((err) => {
    console.log(err)
  })
```

#### sendSingleUserMessage

发送单播消息

```javascript
	chatroom.sendSingleUserMessage({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    content            |     string           | 内容 |
|    receiver           |     string           | 接收者uid |
|    kvExtra            | {[k: string]: string} | 扩展字段key-value |

响应数据：

| Name                  | Type              |  Description |
| --------------------- |   --------------  |  ----------  |
| rescode               | number            | 0：表示成功   |
| msg                   | string            | 返回描述      |


示例：
```javascript
        let options = { content, receiver, kvExtra }
        chatroom.sendSingleUserMessage(options).then((res) => {
          console.log("sendSingleUserMessage Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

#### sendTextChat

发送公屏

```javascript
	chatroom.sendTextChat({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    chat                |     string           | 内容 |
|    extra          |     string          |  扩展字段(新版本已废弃) |
|    kvExtra        |     {[k: string]: string}           | 扩展字段key-value键值对  |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
| rescode               | number            | 0：表示成功   |
| msg                   | string            | 返回描述      |

【注】
（1）新业务接入，扩展字段值请使用kvExtra，不要使用extra；
（2）如果是需要与移动端sdk进行互通的老业务，需要使用extra的情形，请与移动端SDK进行互通联调；


示例：
```javascript
        let params = { chat, extra, kvExtra }
        chatroom.sendTextChat(params).then((res) => {
          console.log("SendTextChat Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

#### getChatRoomAttributes

获取聊天室属性

```javascript
  chatroom.getChatRoomAttributes();
```

请求参数：（无）

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    rescode           |      number          | 0：表示成功|
|    attributes             |      {[k: string]: string}      |  聊天室介绍信息|


示例：
```javascript
  chatroom.getChatRoomAttributes().then(res => {
    console.log("getChatRoomAttributes Res: " + JSON.stringify(res));
  }).catch((err) => {
    console.log(err)
  })
```

响应：
```javascript
{"appid":1504984159,"roomid":0,"rescode":0,"attributes":{"Roomid":"1704994371","Name":"nginx大讲堂","Bulletin":"bull","Owner":"777666","Passwd":"","MaxUser":"100000","AppExtra":"ex","Appid":"1504984159","Description":"全栈技术"}}
```

#### getChatRoomManager

获取聊天室所有管理员

```javascript
  chatroom.getChatRoomManager({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------------- |
|    roler              |    string         | "owner" |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    admins             |      {[k: string]: Array<string>}        | |
|    rescode            |      number          | 0：表示成功|


示例：
```javascript
        let params = { roler }
        chatroom.getChatRoomManager(params).then(res => {
          console.log("GetChatRoomManager Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1504984159,"roomid":1704994371,"admins":{"owner":["777666"]},"rescode":0}
```

#### getUserList

获取聊天室用户列表

```javascript
  chatroom.getUserList({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    num             |      number        |   设置每页的返回成员数（建议不超过200）  |
|    pos             |      number        |  第几页 (第一页从0开始)   |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    pos                |      number          | |
|    uids               |      string[]        | 用户UID列表 |
|    rescode            |      number          | 0：表示成功|

示例：
```javascript
        let params = { num, pos }
        chatroom.getUserList(params).then(res => {
          console.log("GetUserList Res: " + JSON.stringify(res));
        }).catch(err => {
          console.log(err)
        })
```

响应：
```javascript
{"appid":1504984159,"roomid":1704994371,"pos":0,"uids":["777666"],"rescode":0}
```

#### setUserAttributes

设置用户属性

```javascript
  chatroom.setUserAttributes({});
```

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

#### getUserAttributesList

获取用户属性列表

```javascript
  chatroom.getUserAttributesList();
```

请求参数：（无）

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 0：表示成功|
|    users             |      {[k: string]: { [k: string]: string}}   | |


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
{"appid":1504984159,"roomid":1704994373,"users":{"777666":{"name":"张三"},"888999":{"topic":"张三好"}},"rescode":0}
```

#### muteUser

禁言操作

```javascript
  chatroom.muteUser({});
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    uid             |      string          |  被禁言的UID|
|    secs               |      string          | |
|    reason            |      string          | 原因 |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 0：表示成功|
|    users             |      {[k: string]: { [k: string]: string}}   | |


示例：
```javascript
  try {
    let options = { uid, secs, reason };
    const res = await chatroom.muteUser(options);
    console.log("muteUser res=" + JSON.stringify(res));
  } catch (e) {
    console.log("muteUser err=", e);
  };
```

响应：
```javascript
{"appid":1504984159,"roomid":1704994373,"users":{"777666":{"name":"张三"},"888999":{"topic":"张三好"}},"rescode":0}
```


#### getMutedUserList

获取聊天室禁言用户列表

```javascript
  chatroom.getMutedUserList()
```

请求参数：（无）

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    uids               |      string[]        | 禁言用户UID列表 |
|    rescode            |      number          | 0：表示成功|

示例：
```javascript
  chatroom.getMutedUserList().then(res => {
    console.log("getMutedUserList Res: " + JSON.stringify(res));
  }).catch(err => {
    console.log(err)
  })
```

响应：
```javascript
{"appid":1504984159,"roomid":1704994373,"rescode":0,"uids":["123"]}
```

### 接收消息的监听接口
| 监听事件                            | 含义                                                         |
| :-------------------------------- | :----------------------------------------------------------- |
| SingleUserMessage                 | 接收单播消息                                                  |
| GroupMessage                      | 接收广播消息                                                  |
| TextChat                          | 接收公屏消息                                                  |
| ChatRoomAttributesUpdated         | 接收聊天室属性变更通知                                         |
| UserKickedOff                     | 用户被踢的广播通知                                             |
| UserCountUpdated                  | 用户数量更新的通知                                             |
| UserOnlineUpdated                 | 进出聊天室的用户列表通知                                        |
| UserAttributesSet                 | 用户属性设置通知                                               |
| UsersMuted                        | 用户被禁言的通知                                               |
| UsersUnMuted                      | 用户解禁的通知                                                 |
| ChatRoomUserOffline               | 当前用户断线超时离开聊天室回调                                   |



##### 接收单播消息(chatroom.on('SingleUserMessage', (data) => {}))

接收指定房间的消息
```javascript
chatroom.on('SingleUserMessage', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"SingleUserMessage"       |
| handler | function  | 接收回调                 |


handler回调参数：

| Name                  | Type              |  Description |
| --------------------- | --------------- | ----------- |
|    uid                |      string          | 发送者uid|
|    content            |      string          | 消息内容|
|    kvExtra            |     {[k: string]: string}           |  |

示例：
```javascript
 {"uid":"777666","content":"js_sdk sendUnicast","receiver":"777666","requestId":"6834302384034808238","kvExtra":{"name":"nginx大讲堂"}}
```

#### 接收公屏消息(chatroom.on('TextChat', (data) => {}))

```javascript
chatroom.on('TextChat', (data) => { console.log(data); })
```

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | |
|    uid             |      string          |发送者uid|
|    roomid             |      number          ||
|    chat                    |     string           | utf-8 |
|    extra                   |     {[k: string]: string}          |  |
|    kvExtra                    |     {[k: string]: string}           |  |

示例：
```javascript
{"uid":"777666","chat":"js_sdk sendTextChat","extra":{"extra":"extra"},"kvExtra":{"topic":"nginx大讲堂"},"requestId":"6834301645300433320"}
```


#### 接收广播消息(chatroom.on('GroupMessage', (data) => {}))

```javascript
chatroom.on('GroupMessage', (data) => { console.log(data); })
```

| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    uid                |      string          | 发送者uid|
|    content            |      string          | 消息内容|
|    kvExtra            |     {[k: string]: string}           |  |

示例：
```javascript
{"uid":"888999","content":"js_sdk sendGroupMessage","requestId":"6834302753401995276","kvExtra":{"name":"张三"}}
```

#### 接收广播消息(chatroom.on('ChatRoomAttributesUpdated', (data) => {}))

```javascript
chatroom.on('ChatRoomAttributesUpdated', (data) => { console.log(data); })
```

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    uid             |      string          |发送者uid|
|    content             |      string          | 消息内容|

示例：
```javascript
{"uid":"888999","content":"js_sdk sendGroupMessage","requestId":"6834295284453867525","kvExtra":{"name":"张三"}}
```



#### 接收用户被踢的广播通知(chatroom.on('UserKickedOff', (data) => {}))

```javascript
chatroom.on('UserKickedOff', (data) => { console.log(data); })
```

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    admin             |      string          | 发送者uid|
|    tuids             |      string[]          | 被踢者UID列表|
|    secs             |      number          |  踢人code|
|    reason             |      string           | 原因|
|    kicktype           |      number           | 类型|

示例：
```javascript
{"admin":"777666","tuids":["888999"],"secs":3000,"reason":"js KickOffUser","kicktype":0}
```

#### 接收用户数更新消息(chatroom.on('UserCountUpdated', (data) => {}))

```javascript
chatroom.on('UserCountUpdated', (data) => { console.log(data); })
```


| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    totalCount             |      number        | |

示例：
```javascript
{"totalCount":2}
```

#### 接收在线用户变更消息(chatroom.on('UserOnlineUpdated', (data) => {}))

```javascript
chatroom.on('UserOnlineUpdated', (data) => { console.log(data); })
```


| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    joinUsers          |      string[]         | 加入用户列表|
|    leaveUsers         |      string[]         | 离开用户列表|

示例：
```javascript
{"joinUsers":["777666"],"leaveUsers":[]}
```

#### 接收用户属性设置通知(chatroom.on('UserAttributesSet', (data) => {}))

```javascript
chatroom.on('UserAttributesSet', (data) => { console.log(data); })
```


| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    uid                |      string          ||
|    attributes         |      {[k: string]: string}        |   |  |

示例：
```javascript
{"uid":"777666","attributes":{"nick":"awu","name":"张三"}}
```

#### 接收被禁言的广播通知(chatroom.on('UsersMuted', (data) => {}))

```javascript
chatroom.on('UsersMuted', (data) => { console.log(data); })
```


| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    opUid              |      string          | 操作者UID|
|    tuids              |      string[]        |   禁言列表|
|    secs               |      string           |   时间(ms) |
|    reason             |      string           |   原因 |

示例：
```javascript
{"opUid":"777666","tuids":["123"],"secs":"600","reason":""}
```


#### 接收解禁的广播通知(chatroom.on('UsersUnMuted', (data) => {}))

```javascript
chatroom.on('UsersUnMuted', (data) => { console.log(data); })
```


| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    opUid              |      string          | 操作者UID|
|    tuids              |      string[]        |   解禁列表|
|    secs               |      string           |   时间(ms) |
|    reason             |      string           |   原因 |

示例：
```javascript
{"opUid":"777666","tuids":["123"],"secs":"600","reason":""}
```

#### 接收当前用户断线超时离开聊天室回调(chatroom.on('ChatRoomUserOffline', () => {}))

```javascript
chatroom.on('ChatRoomUserOffline', () => {})
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ChatRoomUserOffline"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
|      NA        |                 |                                                  |
