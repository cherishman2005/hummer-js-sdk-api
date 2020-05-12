# Hummer Real-Time Signal Service js-sdk

npm包发布路径： https://www.npmjs.com/package/hummer-rts-sdk


   * [Hummer Real-Time Signal Service js-sdk](#hummer-real-time-signal-service-js-sdk)
      * [js-sdk主要功能](#js-sdk主要功能)
      * [js-sdk对外接口](#js-sdk对外接口)
         * [注意事项](#注意事项)
         * [创建Hummer实例](#创建hummer实例)
            * [VERSION](#version)
            * [初始化Hummer](#初始化hummer)
            * [setLogLevel](#setloglevel)
            * [getState](#getstate)
            * [登录(login)](#登录login)
            * [登出(logout)](#登出logout)
            * [接收连接状态变更的回调通知(hummer.on('ConnectionStateChanged', (data) =&gt; {}))](#接收连接状态变更的回调通知hummeronconnectionstatechanged-data--)
            * [接收Token过期的回调通知(hummer.on('TokenExpired', () =&gt; {}))](#接收token过期的回调通知hummerontokenexpired---)
            * [更新当前Token(refreshToken)](#更新当前tokenrefreshtoken)
         * [初始化Rts Service](#初始化rts-service)
            * [P2P的消息处理](#p2p的消息处理)
               * [发送点对点的消息(sendMessageToUser)](#发送点对点的消息sendmessagetouser)
               * [接收对端消息(client.on('MessageFromUser', (data) =&gt; {}))](#接收对端消息clientonmessagefromuser-data--)
               * [批量查询用户在线列表(queryUsersOnlineStatus)](#批量查询用户在线列表queryusersonlinestatus)
            * [房间消息处理](#房间消息处理)
               * [创建单个房间实例(createRoom)](#创建单个房间实例createroom)
               * [加入房间(join)](#加入房间join)
               * [退出房间(leave)](#退出房间leave)
               * [room发送组播消息(sendMessage)](#room发送组播消息sendmessage)
               * [room设置本地用户属性(setUserAttributes)](#room设置本地用户属性setuserattributes)
               * [room删除本地用户某些属性(deleteUserAttributesByKeys)](#room删除本地用户某些属性deleteuserattributesbykeys)
               * [room添加或更新本地用户的属性(addOrUpdateUserAttributes)](#room添加或更新本地用户的属性addorupdateuserattributes)
               * [room清空本地用户的属性(clearUserAttributes)](#room清空本地用户的属性clearuserattributes)
               * [room获取房间用户列表(getMembers)](#room获取房间用户列表getmembers)
               * [room查询某指定用户指定属性名的属性(getUserAttributesByKeys)](#room查询某指定用户指定属性名的属性getuserattributesbykeys)
               * [room查询某指定用户的全部属性(getUserAttributes)](#room查询某指定用户的全部属性getuserattributes)
               * [room全量设置某指定房间的属性(setRoomAttributes)](#room全量设置某指定房间的属性setroomattributes)
               * [room删除某指定房间的指定属性(deleteRoomAttributesByKeys)](#room删除某指定房间的指定属性deleteroomattributesbykeys)
               * [room更新房间属性(addOrUpdateRoomAttributes)](#room更新房间属性addorupdateroomattributes)
               * [room清空某指定房间的属性(clearRoomAttributes)](#room清空某指定房间的属性clearroomattributes)
               * [room查询某指定房间的全部属性(getRoomAttributes)](#room查询某指定房间的全部属性getroomattributes)
               * [room查询某指定房间指定属性名的属性(getRoomAttributesByKeys)](#room查询某指定房间指定属性名的属性getroomattributesbykeys)
               * [client查询单个或多个房间用户数(getRoomMemberCount)](#client查询单个或多个房间用户数getroommembercount)
               * [room接收组播消息(room.on('RoomMessage', (data) =&gt; {}))](#room接收组播消息roomonroommessage-data--)
               * [room接收到加入房间Notify回调(room.on('MemberJoined', (data) =&gt; {}))](#room接收到加入房间notify回调roomonmemberjoined-data--)
               * [room接收到退出房间Notify回调(room.on('MemberLeft', (data) =&gt; {}))](#room接收到退出房间notify回调roomonmemberleft-data--)
               * [room接收当前用户断线超时离开房间回调(room.on('RoomMemberOffline', () =&gt; {}))](#room接收当前用户断线超时离开房间回调roomonroommemberoffline---)
               * [room接收设置用户属性Notify的回调(room.on('MemberAttributesSet', (data) =&gt; {}))](#room接收设置用户属性notify的回调roomonmemberattributesset-data--)
               * [room接收到删除用户某些属性Notify回调(room.on('MemberAttributesDeleted', (data) =&gt; {}))](#room接收到删除用户某些属性notify回调roomonmemberattributesdeleted-data--)
               * [room接收到清除用户某些属性Notify回调(room.on('MemberAttributesCleared', (data) =&gt; {}))](#room接收到清除用户某些属性notify回调roomonmemberattributescleared-data--)
               * [room接收到添加或更新用户某些属性Notify回调(room.on('MemberAttributesAddedOrUpdated', (data) =&gt; {}))](#room接收到添加或更新用户某些属性notify回调roomonmemberattributesaddedorupdated-data--)
               * [room接收设置房间属性Notify的回调(room.on('RoomAttributesSet', (data) =&gt; {}))](#room接收设置房间属性notify的回调roomonroomattributesset-data--)
               * [room接收到删除房间某些属性Notify回调(room.on('RoomAttributesDeleted', (data) =&gt; {}))](#room接收到删除房间某些属性notify回调roomonroomattributesdeleted-data--)
               * [room接收到清除房间某些属性Notify回调(room.on('RoomAttributesCleared', (data) =&gt; {}))](#room接收到清除房间某些属性notify回调roomonroomattributescleared-data--)
               * [room接收到添加或更新房间某些属性Notify回调(room.on('RoomAttributesAddedOrUpdated', (data) =&gt; {}))](#room接收到添加或更新房间某些属性notify回调roomonroomattributesaddedorupdated-data--)
               * [room接收到房间用户数变更Notify回调(room.on('MemberCountUpdated', (data) =&gt; {}))](#room接收到房间用户数变更notify回调roomonmembercountupdated-data--)
         * [辅助工具](#辅助工具)
            * [getInstanceInfo获取实例信息(getInstanceInfo)](#getinstanceinfo获取实例信息getinstanceinfo)
            * [将string编码成Utf8二进制(encodeStringToUtf8Bytes)](#将string编码成utf8二进制encodestringtoutf8bytes)
            * [将Utf8二进制解码成string类型(decodeUtf8BytesToString)](#将utf8二进制解码成string类型decodeutf8bytestostring)



## js-sdk主要功能

	登录登出、更新Token
	点对点消息
	查询用户在线状态
	加入离开房间相关
	房间消息
	房间内用户属性增删改查
	房间属性增删改查
	查询单个或多个房间的成员人数
	获取房间成员列表


## js-sdk对外接口

### 注意事项

（1）本系统支持使用64位数字的用户ID(uid)。由于javascript的number类型不支持64位的整数。所以sdk使用string来存储uid。

（2）一个浏览器只能登录一个用户（uid）。

RtsService区分可靠P2P；可靠组播；
    

（I）白板功能调试主要场景：（时延较小，满足需求）

    （a）【1对1白板授课】可靠的p2p；

    （b）【1对多白板授课】可靠的组播；

（II）另外一个场景是1对多，老师禁掉其他学生的声音的控制；采用可靠p2p


Hummer RTS JS SDK 是通过 HTML 网页加载的 JavaScript 库。Hummer RTS JS SDK 库在网页浏览器中通过 API 调用Hummer的实时信令服务。


创建Hummer账号并获取AppId。

在编译和启动实例程序前，您需要首先获取一个可用的App ID。

### 创建Hummer实例

#### VERSION

获取Hummer SDK版本号

```javascript
    const version = Hummer.VERSION;
```

#### 初始化Hummer

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

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    NA     | void   |                             |


| 枚举值 | 含义 |
| :--- | :--- |
| DEBUG(-1) | debug |
| LOG(0) | log |
| INFO(1) | info |
| WARN(2) | warn |
| ERROR(3) | error |

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


#### 登录(login)

```javascript
hummer.login({ region, uid, token });
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region?   | string                  |     用户归属地region                                |
| uid       | string                  |     用户UID                                         |
| token?    | string                  |     可选的动态密钥                                  |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


token支持3种模式：appid模式、token模式、临时token模式


#### 登出(logout)

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

（6）登录被踢：（账户互踢功能：后端暂不支持web互踢）
回调通知{"state":"DISCONNECTED","reason":"REMOTE_LOGIN"}


#### 接收Token过期的回调通知(hummer.on('TokenExpired', () => {}))

```javascript
hummer.on('TokenExpired', () => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"TokenExpired" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| NA      |         |  |                |


断链重连时token过期才会回调通知。


#### 更新当前Token(refreshToken)

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


### 初始化Rts Service

初始化： 创建RtsService实例

```javascript
client = hummer.createRTSInstance();
```

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|           | Object | 返回RtsService实例         |


#### P2P的消息处理

##### 发送点对点的消息(sendMessageToUser)

发送P2P的消息

```javascript
client.sendMessageToUser({})；
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array              | 消息的内容。                                        |
| receiver  | string                  | 接收者uid                                           |
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。 |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { type, content, receiver }
client.sendMessageToUser(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err)
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```

##### 接收对端消息(client.on('MessageFromUser', (data) => {}))

接收对端的P2P消息

```javascript
client.on('MessageFromUser', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MessageFromUser"      |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| message | Message | 消息对象。                  |
| fromUid | string  | 发送者的uid                 |

Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| type             | number                  | 消息内容类型                                            |
| data             | Uint8Array              | 消息内容                                                |
| appExtras        | {[k: string]: string}   | 可选参数。 用户自定义的数据。 键和值为string的json-object。  |

```typescript
interface Message {
    message: {
        type: string;
        data: Uint8Array;
        appExtras?: ({ [k: string]: string }|null);
    },
    fromUid: string;
}
```

##### 批量查询用户在线列表(queryUsersOnlineStatus)

批量查询用户在线列表

```javascript
client.queryUsersOnlineStatus({ uids: uids })
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| uids | string[] | 要查询的uid列表   |

响应数据：Promise<> 

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  | 0：表示成功  |
| msg      | string  | 返回描述     |
| onlineStatus | {[uid: string]: boolean} | 用户/在线状态键值对      |

示例：
```js
client.queryUsersOnlineStatus({uids: uids}).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```

响应：
```js
{"rescode":0,"msg":"ok","onlineStatus":{"999000":true}}
```

#### 房间消息处理

##### 创建单个房间实例(createRoom)

创建单个房间实例

```js
room = client.createRoom({ region, roomId })
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 房间ID                      |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    | Object | 返回room实例                |

【注】可以创建多个房间实例，并且可以在不同的业务归属region


##### 加入房间(join)
```js
room.join();
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extra  | {[k: string]: string} | 可扩展字段（选填字段） |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
|  msg     | string  |   返回描述   |

示例：
```js
        let params = {extra};
        room.join(params).then(res => {
          console.log("join res:", res);
        }).catch(err => {
        });
```

##### 退出房间(leave)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extra  | {[k: string]: string} | 可扩展字段（选填字段） |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
|  msg     | string  |   返回描述   |

示例：
```js
        let params = {extra};
        room.leave(params).then(res => {
          console.log("leave res:", res);
        }).catch(err => {
        });
```


##### room发送组播消息(sendMessage)

```js
room.sendMessage({})
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array                  | 消息的内容。                                    |
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的Object。 |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { type, content }
room.sendMessage(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```


##### room设置本地用户属性(setUserAttributes)

```js
room.setUserAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| attributes | {[k: string]: string} | 用户属性 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


示例：
```js
    let params = { attributes };
    room.setUserAttributes(params).then(res => {
      console.log("setUserAttributes Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room删除本地用户某些属性(deleteUserAttributesByKeys)

```js
room.deleteUserAttributesByKeys({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| attributes | {[k: string]: string} | 用户属性 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    room.deleteUserAttributesByKeys({ keys }).then(res => {
      console.log("deleteUserAttributesByKeys res: ", res);
    }).catch(err => {
      console.log(err)
    })
```

##### room添加或更新本地用户的属性(addOrUpdateUserAttributes)

```js
room.addOrUpdateUserAttributes({attributes})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| attributes | string[] | 用户属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    let params = { attributes };
    room.addOrUpdateUserAttributes(params).then(res => {
      console.log("addOrUpdateUserAttributes res:", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room清空本地用户的属性(clearUserAttributes)

```js
room.clearUserAttributes()
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| NA | | | |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    room.clearUserAttributes().then(res => {
      console.log("clearUserAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room获取房间用户列表(getMembers)

```js
room.getMembers()
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    NA        |      NA        |     | |


响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    uids             |      string[]        | 用户列表 |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
        room.getMembers().then(res => {
          console.log("getMembers res:", res);
        }).catch(err => {
        });
```

响应：
```javascript
{"appid":1350626568,"roomId":"test_room","uids":["123","555","233333","1356662","3300235422","3300235423","3300235499","3300235888","135666911222"],"rescode":0}
```

##### room查询某指定用户指定属性名的属性(getUserAttributesByKeys)

```js
room.getUserAttributesByKeys({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    uid                |      string       |  用户UID            |
|    keys               |      string[]     |   用户属性名keys    |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    roomId         |      string        |     |
|    attributes        |      {[k: string]: string}  | 用户属性object |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    room.getUserAttributesByKeys({ uid, keys }).then(res => {
      console.log("getUserAttributesByKeys res:", res);
    }).catch(err => {
    });
```


##### room查询某指定用户的全部属性(getUserAttributes)

```js
room.getUserAttributes({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    uid                |      string       |  用户UID            |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    roomId         |      string        |     |
|    attributes        |      {[k: string]: string}  | 用户属性object |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    room.getUserAttributes({ uid, keys }).then(res => {
      console.log("getUserAttributes res:", res);
    }).catch(err => {
    });
```

##### room全量设置某指定房间的属性(setRoomAttributes)

```js
room.setRoomAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| attributes | {[k: string]: string} | 房间属性key-value键值对 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


示例：
```js
    let params = { attributes };
    room.setRoomAttributes(params).then(res => {
      console.log("setRoomAttributes res:", res);
    }).catch(err => {
      console.log(err)
    })
```

全量设置某指定房间的属性。

Note

你无需加入指定房间即可为该房间设置房间属性。
当某房间处于空房间状态（无人状态）数分钟后，该房间的房间属性将被清空。


##### room删除某指定房间的指定属性(deleteRoomAttributesByKeys)

```js
room.deleteRoomAttributesByKeys({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| keys | string[] | 房间属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    room.deleteroomAttributesByKeys({ keys }).then(res => {
      console.log("deleteroomAttributesByKeys Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```

##### room更新房间属性(addOrUpdateRoomAttributes)

```js
room.addOrUpdateRoomAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| attributes | {[k: string]: string} | 房间属性key-value键值对 |

响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    let params = { attributes };
    room.addOrUpdateRoomAttributes(params).then(res => {
      console.log("addOrUpdateRoomAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room清空某指定房间的属性(clearRoomAttributes)

```js
room.clearRoomAttributes()
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| NA   |        |   |         |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    room.clearRoomAttributes().then(res => {
      console.log("clearRoomAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room查询某指定房间的全部属性(getRoomAttributes)

```js
room.getRoomAttributes()
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| NA   |        | |           |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |
| attributes | {[k: string]: string} | 房间属性key-value键值对 |

示例：
```js
    room.getRoomAttributes().then(res => {
      console.log("getRoomAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room查询某指定房间指定属性名的属性(getRoomAttributesByKeys)

```js
room.getRoomAttributesByKeys()
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| keys | string[] | 房间属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |
| attributes | {[k: string]: string} | 房间属性key-value键值对 |

示例：
```js
    room.getRoomAttributesByKeys({ keys }).then(res => {
      console.log("getRoomAttributesByKeys res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### client查询单个或多个房间用户数(getRoomMemberCount)

在预先设定的区域，查询单个或多个房间用户数

```js
client.getRoomMemberCount({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region?    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
|    roomIds        |      string[]        |   同一区域的房间列表       |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    userCount       |      {[roomId: string]: number}     |     |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    client.getRoomMemberCount({ region, roomIds }).then(res => {
      console.log("res:", res);
    }).catch(err => {
    });
```

响应示例：
```javascript
{"appid":1350626568,"userCount":{"test123":0,"test999":3},"rescode":0,"msg":""}
```


##### room接收组播消息(room.on('RoomMessage', (data) => {}))
接收指定房间的消息
```javascript
room.on('RoomMessage', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomMessage"       |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| message | Message | 房间消息对象                |
| fromUid | string  | 发送者的uid                 |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| type             | number                  | 消息内容类型                                            |
| data             | Uint8Array              | 消息内容                                                |
| appExtras        | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。  |

```typescript
interface RoomMessage {
    message: {
        type: string;
        data: Uint8Array;
        appExtras?: ({ [k: string]: string }|null);
    },
    fromUid: string;
}
```

##### room接收到加入房间Notify回调(room.on('MemberJoined', (data) => {}))

接收加入房间的用户列表通知。

```
room.on('MemberJoined', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberJoined"    |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义见data定义          |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |

```javascript
{"uid":["555"]}
```

##### room接收到退出房间Notify回调(room.on('MemberLeft', (data) => {}))

接收退出房间的用户列表通知。

```
room.on('MemberLeft', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberLeft"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |

参考示例：

```javascript
{"uid":["135666911222"]}
```

##### room接收当前用户断线超时离开房间回调(room.on('RoomMemberOffline', () => {}))

接收退出房间的用户列表通知。

```javascript
room.on('RoomMemberOffline', () => {})
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomMemberOffline"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
|      NA        |                 |                                                  |


##### room接收设置用户属性Notify的回调(room.on('MemberAttributesSet', (data) => {}))

接收到该房间设置用户某些属性Notify回调通知。

```javascript
room.on('MemberAttributesSet', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberAttributesSet" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义见data定义          |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes       | object                  | 用户属性 |


参考示例：

```javascript
{"uid":"123","attributes":{"Name":"阿武","Description":"js_sdk测试","Bulletin":"bull","Extention":"ex"}}
```

##### room接收到删除用户某些属性Notify回调(room.on('MemberAttributesDeleted', (data) => {}))

接收到该房间删除用户某些属性Notify回调通知

```
room.on('MemberAttributesDeleted', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberAttributesDeleted" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes       | {[k:string]: string}    | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"999888","attributes":{"room_role_name":"student"}}
```

##### room接收到清除用户某些属性Notify回调(room.on('MemberAttributesCleared', (data) => {}))

接收到该房间清除用户某些属性Notify回调通知

```
room.on('MemberAttributesCleared', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberAttributesCleared" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes       | {[k:string]: string}    | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"999888","attributes":{"room_role_name":"student"}}
```

##### room接收到添加或更新用户某些属性Notify回调(room.on('MemberAttributesAddedOrUpdated', (data) => {}))

接收到添加或更新用户某些属性Notify回调通知

```
room.on('MemberAttributesAddedOrUpdated', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberAttributesAddedOrUpdated" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |

data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes       | {[k:string]: string}    | 用户属性 |


参考示例：

```javascript
{"uid":"999888","attributes":{"Name":"awu","room_role_name":"student"}}
```


##### room接收设置房间属性Notify的回调(room.on('RoomAttributesSet', (data) => {}))

接收设置房间属性Notify的回调通知。

```
room.on('RoomAttributesSet', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomAttributesSet" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |

data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更房间属性的用户UID |
| attributes       | {[k:string]: string}    | 房间属性 |

参考示例：

```javascript
{"lastUpdateUid":"999888","attributes":{"Name":"awu","Bulletin":"bull","Extention":"ex","room_name":"nginx大讲堂"}}
```

##### room接收到删除房间某些属性Notify回调(room.on('RoomAttributesDeleted', (data) => {}))

接收到该房间删除用户某些属性Notify回调通知

```
room.on('RoomAttributesDeleted', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomAttributesDeleted" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更房间属性的用户UID |
| attributes       | {[k:string]: string}    | 房间属性 |


##### room接收到清除房间某些属性Notify回调(room.on('RoomAttributesCleared', (data) => {}))

接收到该房间清除用户某些属性Notify回调通知

```
room.on('RoomAttributesCleared', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomAttributesCleared" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更房间属性的用户UID |
| attributes       | {[k:string]: string}    | 房间属性 |




参考示例：

```javascript
{"lastUpdateUid":"999888","attributes":{"room_name":"nginx大讲堂"}}
```


##### room接收到添加或更新房间某些属性Notify回调(room.on('RoomAttributesAddedOrUpdated', (data) => {}))

接收到添加或更新房间某些属性Notify回调通知

```
room.on('RoomAttributesAddedOrUpdated', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomAttributesAddedOrUpdated" |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更房间属性的用户UID |
| attributes       | {[k:string]: string}    | 房间属性 |


参考示例：

```javascript
{"lastUpdateUid":"999888","attributes":{"room_name":"nginx大讲堂","owner":"awu"}}
```

##### room接收到房间用户数变更Notify回调(room.on('MemberCountUpdated', (data) => {}))

接收到添加或更新房间某些属性Notify回调通知

```
room.on('MemberCountUpdated', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"MemberCountUpdated" |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| count    | number                  | 房间内用户数 |


参考示例：

```javascript
{"count":1}
```


### 辅助工具

#### getInstanceInfo获取实例信息(getInstanceInfo)

获取实例信息的API接口，用来辅助排查实例的在线状态信息，方便用于调试。

请求参数：

| Name | Type |
| ---- | ---- |
| NA   |      |


响应数据：

| Name       | Type   | Description |
| ---------- | ------ | ----------- |
| appid      | number |             |
| uid        | string | UID         |
| instanceId | number | 实例ID      |

示例：
```javascript
hummer.getInstanceInfo().then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
});
```
响应：
```javascript
{ "uid": "123", "instanceId": 3114848827, "isAnonymous": false, "appid": 1350626568 }
```

#### 将string编码成Utf8二进制(encodeStringToUtf8Bytes)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
|      | string |             |

响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|          | Utf8Bytes |           |


示例：
```javascript
Hummer.Utify.encodeStringToUtf8Bytes(content);
```

#### 将Utf8二进制解码成string类型(decodeUtf8BytesToString)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
|      | Utf8Bytes |        |     |

响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|          | string |     |      |

示例：
```javascript
Hummer.Utify.decodeUtf8BytesToString(content);
```
