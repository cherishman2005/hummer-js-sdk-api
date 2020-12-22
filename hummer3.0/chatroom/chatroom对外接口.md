<!-- TOC -->

- [Hummer chatroom js-sdk](#hummer-chatroom-js-sdk)
    - [js-sdk对外接口](#js-sdk对外接口)
        - [注意事项](#注意事项)
        - [createHummer](#createhummer)
            - [setLogLevel](#setloglevel)
            - [getConnectionState](#getconnectionstate)
            - [login](#login)
            - [logout](#logout)
            - [接收连接状态变更的回调通知(hummer.on('ConnectionStateChanged', (data) => {}))](#接收连接状态变更的回调通知hummeronconnectionstatechanged-data--)
            - [接收登录互踢的回调通知(hummer.on('ForceoutOffline', (data) => {}))](#接收登录互踢的回调通知hummeronforceoutoffline-data--)
            - [TokenExpired](#tokenexpired)
            - [TokenWillExpire](#tokenwillexpire)
            - [refreshToken](#refreshtoken)
            - [createChatRoom](#createchatroom)
            - [dismissChatRoom](#dismisschatroom)
            - [getChatRoomUserCount](#getchatroomusercount)
        - [ChatRoom](#chatroom)
            - [createChatRoomInstance](#createchatroominstance)
            - [joinChatRoom](#joinchatroom)
            - [leaveChatRoom](#leavechatroom)
            - [sendGroupMessage](#sendgroupmessage)
            - [sendSingleUserMessage](#sendsingleusermessage)
            - [sendTextChat](#sendtextchat)
            - [getChatRoomAttributes](#getchatroomattributes)
            - [updateChatRoomAttributes](#updatechatroomattributes)
            - [getChatRoomManager](#getchatroommanager)
            - [getUserList](#getuserlist)
            - [setUserAttributes](#setuserattributes)
            - [getUserAttributesList](#getuserattributeslist)
            - [muteUser](#muteuser)
            - [unMuteUser](#unmuteuser)
            - [getMutedUserList](#getmuteduserlist)
            - [fetchHistoryMessages](#fetchhistorymessages)
            - [setRoomExtraAttributes](#setroomextraattributes)
            - [updateRoomExtraAttributes](#updateroomextraattributes)
            - [deleteRoomExtraAttributes](#deleteroomextraattributes)
            - [clearRoomExtraAttributes](#clearroomextraattributes)
            - [fetchRoomExtraAttributes](#fetchroomextraattributes)
        - [接收消息的监听接口](#接收消息的监听接口)
                - [接收单播消息(chatroom.on('SingleUserMessage', (data) => {}))](#接收单播消息chatroomonsingleusermessage-data--)
            - [TextChat](#textchat)
            - [GroupMessage](#groupmessage)
            - [接收广播消息(chatroom.on('ChatRoomAttributesUpdated', (data) => {}))](#接收广播消息chatroomonchatroomattributesupdated-data--)
            - [接收用户被踢的广播通知(chatroom.on('UserKickedOff', (data) => {}))](#接收用户被踢的广播通知chatroomonuserkickedoff-data--)
            - [接收用户数更新消息(chatroom.on('UserCountUpdated', (data) => {}))](#接收用户数更新消息chatroomonusercountupdated-data--)
            - [接收在线用户变更消息(chatroom.on('UserOnlineUpdated', (data) => {}))](#接收在线用户变更消息chatroomonuseronlineupdated-data--)
            - [接收用户属性设置通知(chatroom.on('UserAttributesSet', (data) => {}))](#接收用户属性设置通知chatroomonuserattributesset-data--)
            - [接收被禁言的广播通知(chatroom.on('UsersMuted', (data) => {}))](#接收被禁言的广播通知chatroomonusersmuted-data--)
            - [接收解禁的广播通知(chatroom.on('UsersUnMuted', (data) => {}))](#接收解禁的广播通知chatroomonusersunmuted-data--)
            - [ChatRoomUserOffline](#chatroomuseroffline)
            - [RoomExtraAttributesSet](#roomextraattributesset)
            - [RoomExtraAttributesUpdated](#roomextraattributesupdated)
            - [RoomExtraAttributesDeleted](#roomextraattributesdeleted)
            - [RoomExtraAttributesCleared](#roomextraattributescleared)
        - [消息通道](#消息通道)
            - [createMessage](#createmessage)
            - [P2P消息](#p2p消息)
                - [fetchUserOnlineStatus](#fetchuseronlinestatus)
                - [sendP2PMessage](#sendp2pmessage)
                - [P2PMessageReceived](#p2pmessagereceived)
            - [P2C Channel消息](#p2c-channel消息)
                - [创建channel实例](#创建channel实例)
                - [joinChannel](#joinchannel)
                - [leaveChannel](#leavechannel)
                - [sendP2CMessage](#sendp2cmessage)
                - [P2CMessageReceived](#p2cmessagereceived)

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

#### getConnectionState

获取SDK当前状态

getState更名为getConnectionState

```javascript
hummer.getConnectionState();
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
| UNAVAILABLE  |通道还未初始化|
| DISCONNECTED | 通道处于断开连接的状态 |
| CONNECTING   | 通道处于连接中的状态 |
| RECONNECTING | 重连中 |
| CONNECTED    | 通道处于连接成功的状态 |


#### login

登录

```javascript
hummer.login({ region, uid, token });
```

**请求参数：**

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

**请求参数：**

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
|   NA      |                         |                 |                                   |


**响应数据：Promise<>**

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
| handler   | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| state   | enum    | 新的连接状态                |
| reason  | enum    | 状态改变的原因              |

```typescript
enum ConnectionState {
    UNAVAILABLE = "UNAVAILABLE",
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


#### TokenExpired

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

#### TokenWillExpire

接收token即将过期的回调通知

```javascript
hummer.on('TokenWillExpire', () => {});
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string    | 取值"TokenWillExpire"       |
| handler   | function  | 接收回调                 |

**回调参数：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| NA      |         |  只用监听事件，data为空       |

【注】
* 接收token即将过期的回调通知。


#### refreshToken

更新当前Token

```javascript
hummer.refreshToken({ token });
```

**请求参数：**

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| token     | string                  |           动态密钥                                  |

**响应数据：Promise<>**

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


#### createChatRoom

创建聊天室

```javascript
  hummer.createChatRoom({})
```

**请求参数：**


| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region    | string                  | chatroom归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| attributes        |      {[k: string]: string}       |   房间属性       |


**响应数据：Promise<>**

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

#### dismissChatRoom

解散聊天室。

```javascript
  hummer.dismissChatRoom()
```

**请求参数**

（无）

**响应数据：Promise<>**

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
| rescode               | number            | 返回码：0-成功  |
| msg                   | string            | 返回描述       |


**请求示例**
```javascript
hummer.dismissChatRoom().then((res) => {
  console.log("dismissChatRoom res:", res);
}).catch((err) => {
  console.log(err)
});
```

**响应示例**
```javascript
{"appid":1350626568,"uid":"777888","roomid":200637135,"rescode":0,"props":{},"msg":"OK"}
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

**请求参数：**

| Name                  | Type              |  Description                 |
| --------------------- | ----------------- |  ------------  |
|    joinProps          |  {[k: string]: string} | 扩展字段，用于业务扩展，SDK 只负责透传 |


**响应数据：Promise<>**

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

**请求参数：**
（无）

**响应数据：**

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

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| content               | string            | 内容            |
| kvExtra               | {[k: string]: string}  | 扩展字段key-value |


**响应数据：**

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

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    content            |     string           | 内容 |
|    receiver           |     string           | 接收者uid |
|    kvExtra            | {[k: string]: string} | 扩展字段key-value |

**响应数据：**

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

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    chat                |     string           | 内容 |
|    extra          |     string          |  扩展字段(新版本已废弃) |
|    kvExtra        |     {[k: string]: string}           | 扩展字段key-value键值对  |


**响应数据：**

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

**请求参数：**

（无）

**响应数据：Promise<>**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    rescode            |      number          | 0：表示成功 |
|    attributes         |      {[k: string]: string}  |  聊天室介绍信息 |

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

#### updateChatRoomAttributes

更新聊天室属性

```javascript
  chatroom.updateChatRoomAttributes({});
```

**请求参数**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    attributes         |      {[k: string]: string}        | 聊天室基础属性 |

**响应数据：Promise<>**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ----------- |
|    rescode           |      number          | 返回码：0-成功 |
|    msg               |      string          |  提示         |


**请求示例**
```javascript
  let attributes = { "Name": "awu", "Description": "js_sdk测试", "Bulletin": "bull", "Extention": "ex" };

  let params = { attributes };
  chatroom.updateChatRoomAttributes(params).then((res) => {
    console.log("updateChatRoomAttributes Res: ", res);
  }).catch((err) => {
    console.log(err)
  })
```

#### getChatRoomManager

获取聊天室所有管理员

```javascript
  chatroom.getChatRoomManager({});
```

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------------- |
|    roler              |    string         | "owner" |


**响应数据：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    admins             |      {[k: string]: Array<string>}        | |
|    rescode            |      number          | 0：表示成功|
|    msg                |      string          | 返回描述      |

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

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    num             |      number        |   设置每页的返回成员数（建议不超过200）  |
|    pos             |      number        |  第几页 (第一页从0开始)   |


**响应数据：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    pos                |      number          | |
|    uids               |      string[]        | 用户UID列表 |
|    rescode            |      number          | 0：表示成功|
|    msg                |      string          | 返回描述      |

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

**请求参数：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    attributes         |      {[k: string]: string}        |   |  |


**响应数据：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 0：表示成功|
|    msg                |      string          | 返回描述      |


示例：
```javascript
        let attributes = { "name": "awu", "description": "js_sdk测试"};

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

**请求参数：**

（无）

**响应数据：**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 0：表示成功|
|    users             |      {[k: string]: { [k: string]: string}}   | |
|    msg                |      string          | 返回描述      |

**示例：**
```javascript
        chatroom.getUserAttributesList().then((res) => {
          console.log("getUserAttributesList Res: " + JSON.stringify(res));
        }).catch((err) => {
          console.log(err)
        })
```

**响应：**
```javascript
{"appid":1504984159,"roomid":1704994373,"users":{"777666":{"name":"张三"},"888999":{"topic":"张三好"}},"rescode":0}
```

#### muteUser

禁言操作。

```javascript
  chatroom.muteUser({});
```

**请求参数**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    uid             |      string          |  被禁言用户ID|
|    secs               |      string          | 预留字段 |
|    reason            |      string          | 原因 |


**响应数据**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 返回码：0-成功 |
|    msg                |      string          | 返回描述      |


**请求示例**
```javascript
  try {
    let options = { uid, secs, reason };
    const res = await chatroom.muteUser(options);
    console.log("muteUser res=" + JSON.stringify(res));
  } catch (e) {
    console.log("muteUser err=", e);
  };
```

**响应示例**
```javascript
{"appid":1350626568,"uid":"777888","roomid":200637135,"rescode":0,"props":{},"msg":"OK"}
```

#### unMuteUser

解禁操作。

```javascript
  chatroom.unMuteUser({});
```

**请求参数**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    uid             |      string          |  解禁用户ID|
|    secs               |      string          | 预留字段 |
|    reason            |      string          | 原因 |


**响应数据**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |  ---------  |
|    rescode           |      number          | 返回码：0-成功 |
|    msg                |      string          | 返回描述      |


**请求示例**
```javascript
  try {
    let options = { uid, secs, reason };
    const res = await chatroom.unMuteUser(options);
    console.log("unMuteUser res=" + JSON.stringify(res));
  } catch (e) {
    console.log("unMuteUser err=", e);
  };
```

**响应示例**
```javascript
{"appid":1350626568,"uid":"777888","roomid":200637135,"rescode":0,"props":{},"msg":"OK"}
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
|    rescode            |      number          | 0：表示成功   |
|    msg                |      string          | 返回描述      |

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

#### fetchHistoryMessages

chatroom历史消息获取：查询历史消息
```js
chatroom.fetchHistoryMessages({})
```

**请求参数**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| msgTypes | number[] | 消息类型列表 |
| anchor   | { uuid: string, timestamp: number } | 消息锚点. 为null则表示从最新消息开始查询 |
| direction | number | 查询方向 enum HistoryDirection  |
| limit     | number |消息条数: 每次最多查询100条. limit>100时, 当100处理; <1时, 当1处理  |

```
enum Direction {
    OLD = 0; // 基于锚点消息，查询更早于锚点消息的消息
    NEW = 1; // 基于锚点消息，查询更晚于锚点消息的消息
}
```

【注】
msgTypes暂支持Text(0)

**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功      |
| msg      | string  | 返回描述      |
| hasMore  | bealean  |  是否还有消息，true: 有，false: 无    |
| messages | Message[]  |  消息列表    |


```
interface Message {
    msgType: MsgType;
    content: string;
    uuid: string;
    timestamp: number;
    uid: string;
    extra: string;
    kvExtra: {[k: string]: string};
}
```

【注】
* limit < 1当1处理
* 1 <= limit <= 100 按实际值处理
* limit > 100 当100处理

#### setRoomExtraAttributes

设置房间扩展属性
```js
chatroom.setRoomExtraAttributes({})
```

**请求参数：**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraAttributes | {[k: string]: string} | 房间属性key-value键值对 |


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


示例：
```js
try {
    let params = { extraAttributes };
    const res = await chatroom.setRoomExtraAttributes(params);
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```


#### updateRoomExtraAttributes

更新房间扩展属性
```js
chatroom.updateRoomExtraAttributes({})
```

**请求参数：**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraAttributes | {[k: string]: string} | 房间属性key-value键值对 |


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


**示例：**
```js
try {
    let params = { extraAttributes };
    const res = await chatroom.updateRoomExtraAttributes(params);
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

#### deleteRoomExtraAttributes

删除房间的指定扩展属性
```js
chatroom.deleteRoomExtraAttributes({})
```

**请求参数：**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraKeys | string[] | 房间属性key数组 |


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  | 返回描述      |

**示例：**
```js
try {
    const res = await chatroom.deleteRoomExtraAttributes({extraKeys});
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

#### clearRoomExtraAttributes

清空某指定房间的扩展属性
```js
chatroom.clearRoomExtraAttributes()
```

**请求参数：**
（无）


**响应数据：Promise<>
**
| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  |  返回描述      |

示例：
```js
try {
    const res = await chatroom.clearRoomExtraAttributes();
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

#### fetchRoomExtraAttributes

查询房间的指定扩展属性

```js
chatroom.fetchRoomExtraAttributes({})
```

**请求参数：**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraKeys | string[] | 房间属性key数组 |


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  | 返回描述      |

**示例：**
```js
try {
    const res = await chatroom.fetchRoomExtraAttributes({extraKeys});
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

### 接收消息的监听接口

| 监听事件                           | 含义                                                         |
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
| RoomExtraAttributesSet            | 接收设置房间扩展属性Notify的回调通知。                           |
| RoomExtraAttributesUpdated        | 接收到添加或更新房间某些扩展属性Notify回调通知                    |
| RoomExtraAttributesDeleted        | 接收到删除房间某些扩展属性Notify回调                             |
| RoomExtraAttributesCleared        | 接收到清除房间某些扩展属性Notify回调。                           |


##### 接收单播消息(chatroom.on('SingleUserMessage', (data) => {}))

接收指定房间的消息
```javascript
chatroom.on('SingleUserMessage', (data) => { console.log(data); })
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"SingleUserMessage"       |
| handler | function  | 接收回调                 |


**handler回调参数：**

| Name                  | Type              |  Description |
| --------------------- | --------------- | ----------- |
|    uid                |      string          | 发送者uid|
|    content            |      string          | 消息内容|
|    kvExtra            |     {[k: string]: string}           |  |

示例：
```javascript
 {"uid":"777666","content":"js_sdk sendUnicast","receiver":"777666","requestId":"6834302384034808238","kvExtra":{"name":"nginx大讲堂"}}
```

#### TextChat

接收公屏消息。

```javascript
chatroom.on('TextChat', (data) => { console.log(data); })
```

| 参数                  | 类型              |  说明 |
| --------------------- | ----------------- | ----------- |
|    appid             |      number          | AppID |
|    uid             |      string          |发送者ID|
|    roomid             |      number          |房间ID|
|    chat                    |     string           | utf-8 |
|    extra                   |     {[k: string]: string}          | 附加信息（废弃） |
|    kvExtra                    |     {[k: string]: string}           | 附加信息 |

【注】尽量不要用 extra字段（新版本已经废弃，只是为了兼容才保留），请使用kvExtra字段。

**示例**

```javascript
{"uid":"777888","chat":"js_sdk sendTextChat","extra":{"extra":""},"kvExtra":{"sss":"你好"}}
```

#### GroupMessage

接收广播消息。

```javascript
chatroom.on('GroupMessage', (data) => { console.log(data); })
```

| Name                  | Type              |  Description |
| --------------------- | ----------------- | -----------  |
|    uid                |      string          | 发送者uid|
|    content            |      string          | 消息内容|
|    kvExtra            |     {[k: string]: string}           |  |

**示例：**
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

**示例：**
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

**示例：**
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

**示例：**
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

**示例：**
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

**示例：**
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

**示例：**
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

**示例：**
```javascript
{"opUid":"777666","tuids":["123"],"secs":"600","reason":""}
```

#### ChatRoomUserOffline

接收当前用户断线超时离开聊天室回调
```javascript
chatroom.on('ChatRoomUserOffline', () => {})
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ChatRoomUserOffline"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name             | type                    | description                    |
| ---------------- | ----------------------- | ------------------------------ |
|      NA        |                 |                                          |


#### RoomExtraAttributesSet

接收设置房间扩展属性Notify的回调通知。

```
chatroom.on('RoomExtraAttributesSet', (data) => { console.log(data); })
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomAttributesSet" |
| handler | function  | 接收回调                 |

**handler回调参数：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |

**data定义：**

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更房间属性的用户UID |
| attributes       | {[k:string]: string}    | 房间属性 |

#### RoomExtraAttributesUpdated

接收到添加或更新房间某些扩展属性Notify回调通知

```
chatroom.on('RoomExtraAttributesUpdated', (data) => { console.log(data); });
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomExtraAttributesUpdated" |
| handler | function  | 接收回调                 |


**handler回调参数：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |



**data定义：**

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |

#### RoomExtraAttributesDeleted

接收到删除房间某些扩展属性Notify回调

```
chatroom.on('RoomExtraAttributesDeleted', (data) => { console.log(data); });
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomExtraAttributesDeleted" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


**data定义：**

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |


#### RoomExtraAttributesCleared

接收到清除房间某些扩展属性Notify回调。

```
chatroom.on('RoomExtraAttributesCleared', (data) => { console.log(data); });
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomExtraAttributesCleared" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


**data定义：**

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |

### 消息通道

#### createMessage

创建message对象

Text文本对象TextMessage构造消息对象

| Name       | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| msgType    | MsgType(enum)         |  消息类型             |
| content    | string                |  消息内容             |

```
interface TextMessage {
    msgType: MessageType;
    content: string;
};
```

**MsgType类型定义：**

| 枚举值 | 含义 |
| :--- | :--- |
| TEXT(0)  | 文本类型 |
| IMAGE(1) | 图片类型 |
| BINARY(2) | 二进制类型 |

```
export enum MsgType {
    TEXT = 0,
    IMAGE = 1,
    BINARY = 2,
}
```

**示例：**
```
let content = 'js-sdk message';
let message: TextMessage = Hummer.createMessage(MsgType.TEXT, content);
```

【注】 
暂支持 文本消息（TEXT：msgType=0）


#### P2P消息

##### fetchUserOnlineStatus

批量查询用户在线状态
```javascript
hummer.fetchUserOnlineStatus({ uids })
```

**请求参数：**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| uids | string[] | 要查询的uid列表   |

**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  | 0：表示成功  |
| msg      | string  | 返回描述     |
| status | {[uid: string]: OnlineStatus} | 用户/在线状态键值对      |

**OnlineStatus定义：**
```
enum OnlineStatus {
    UNDEFINED = -1,
    OFFLINE = 0,
    ONLINE = 1,
    DISCONNECTED = 2,
};
```

**示例：**
```js
try {
    const res = await this.hummer.fetchUserOnlineStatus({uids});
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.log(err);
}
```

##### sendP2PMessage

发送点对点（P2P）的消息

```javascript
hummer.sendP2PMessage({})；
```

**请求参数**

| Name      | Type                    | Description                                  |
| --------- | ----------------------- | -------------------------------------------- |
| recevier  | string                  | 接收者UID                                     |
| message   | TextMessage              | 消息对象Object                                  |
| options  | {isOffline： boolean}  | 离线、在线选项 true:支持离线；false：支持在线消息     |
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的Object。 |

**响应数据：Promise<>**

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

**示例：**
```javascript
try {
    let params = { uid, message, options }
    const res = await hummer.sendP2PMessage(params);
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.error(e);
}
```

##### P2PMessageReceived

接收对端的P2P消息
```javascript
hummer.on(eventName: 'P2PMessageReceived', handler: (data) => { console.log(data); });
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"P2PMessageReceived"   |
| handler | function  | 接收回调                 |


**handler回调参数：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object | 详细定义将data定义描述       |


**data定义：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| opUid  | string    | 发送者用户ID                 |
| message | Message  | 详见构造消息对象（Message）   |


#### P2C Channel消息

##### 创建channel实例

```javascript
channel = hummer.createChannel({region, channelId})
```

**请求参数**

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region    | string                  | channel归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| channelId             |      string      |   channel ID       |


**响应数据**

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|                       | object            | channel实例   |

##### joinChannel 

加入channel

```js
channel.joinChannel();
```

**请求参数：（无）**


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
|  msg     | string  |   返回描述   |

示例：
```js
try {
    const res = await channel.joinChannel();
    console.log("joinChannel res=", res);
} catch(e) {
    console.error(e);
}

```

##### leaveChannel

离开channel
```js
channel.leaveChannel();
```

**请求参数：（无）**


**响应数据：Promise<>**

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  |   返回描述    |

**示例：**
```js
try {
    const res = await channel.leaveChannel();
    console.log("leaveChannel res:", res);
} catch(e) {
    console.error(e);
}
```

##### sendP2CMessage

发送channel（P2C）消息

```js
channel.sendP2CMessage({})
```

**请求参数：**

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| message   | TextMessage              | 文本消息对象。                                |
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的Object。 |

**响应数据：Promise<>**

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

**示例：**
```javascript
try {
    let params = { message }
    const res = await channel.sendP2CMessage(params);
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.error(e);
}
```

【注】
channel暂不支持离线消息

##### P2CMessageReceived

监听channel（P2C）消息

```javascript
channel.on('P2CMessageReceived', (data) => { console.log(data); })
```

**回调通知：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"P2CMessageReceived"    |
| handler | function  | 接收回调                 |


**handler回调参数：**

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| opUid   | string  | 发送者用户ID                 |
| message | Message | Message消息对象              |
