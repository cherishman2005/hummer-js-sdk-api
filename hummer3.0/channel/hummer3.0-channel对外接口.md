# Hummer Channel Service js-sdk

npm包发布路径： https://www.npmjs.com/package/hummer-channel-sdk


   * [Hummer Channel Service js-sdk](#hummer-channel-service-js-sdk)
      * [js-sdk主要功能](#js-sdk主要功能)
      * [js-sdk对外接口](#js-sdk对外接口)
         * [注意事项](#注意事项)
         * [初始化Hummer](#初始化hummer)
            * [登录(login)](#登录login)
            * [登出(logout)](#登出logout)
            * [接收连接状态变更的回调通知(hummer.on('ConnectionStateChange', (data) =&gt; {}))](#接收连接状态变更的回调通知hummeronconnectionstatechange-data--)
            * [接收Token过期的回调通知(hummer.on('TokenExpired', () =&gt; {}))](#接收token过期的回调通知hummerontokenexpired---)
            * [更新当前Token(refreshToken)](#更新当前tokenrefreshtoken)
            * [设置用户归属地(setUserRegion)](#设置用户归属地setuserregion)
         * [初始化room Service](#初始化room-service)
            * [P2P的消息处理](#p2p的消息处理)
               * [发送P2P的消息(sendMessageToUser)](#发送p2p的消息sendmessagetouser)
               * [接收对端消息(client.on('MessageFromUser', (data) =&gt; {}))](#接收对端消息clientonmessagefromuser-data--)
               * [批量查询用户在线列表(queryUsersOnlineStatus)](#批量查询用户在线列表queryusersonlinestatus)
            * [频道消息处理](#频道消息处理)
               * [创建单个频道实例(createRoom)](#创建单个频道实例createroom)
               * [加入频道(joinRoom)](#加入频道joinroom)
               * [退出频道(leaveRoom)](#退出频道leaveroom)
               * [room发送组播消息(sendMessageToRoom)](#room发送组播消息sendmessagetoroom)
               * [room设置本地用户属性(setLocalUserAttributes)](#room设置本地用户属性setlocaluserattributes)
               * [room删除本地用户某些属性(deleteLocalUserAttributesByKeys)](#room删除本地用户某些属性deletelocaluserattributesbykeys)
               * [room添加或更新本地用户的属性(addOrUpdateLocalUserAttributes)](#room添加或更新本地用户的属性addorupdatelocaluserattributes)
               * [room清空本地用户的属性(clearLocalUserAttributes)](#room清空本地用户的属性clearlocaluserattributes)
               * [room获取频道用户列表(getRoomUserList)](#room获取频道用户列表getroomuserlist)
               * [room查询某指定用户指定属性名的属性(getUserAttributesByKeys)](#room查询某指定用户指定属性名的属性getuserattributesbykeys)
               * [room查询某指定用户的全部属性(getUserAttributes)](#room查询某指定用户的全部属性getuserattributes)
               * [client全量设置某指定频道的属性(setRoomAttributes)](#client全量设置某指定频道的属性setroomattributes)
               * [client删除某指定频道的指定属性(deleteRoomAttributesByKeys)](#client删除某指定频道的指定属性deleteroomattributesbykeys)
               * [client更新频道属性(addOrUpdateRoomAttributes)](#client更新频道属性addorupdateroomattributes)
               * [client清空某指定频道的属性(clearRoomAttributes)](#client清空某指定频道的属性clearroomattributes)
               * [client查询某指定频道的全部属性(getRoomAttributes)](#client查询某指定频道的全部属性getroomattributes)
               * [client查询某指定频道指定属性名的属性(getRoomAttributesByKeys)](#client查询某指定频道指定属性名的属性getroomattributesbykeys)
               * [client查询单个或多个频道用户数(getRoomUserCount)](#client查询单个或多个频道用户数getroomusercount)
               * [room接收组播消息(room.on('RoomMessage', (data) =&gt; {}))](#room接收组播消息roomonroommessage-data--)
               * [room接收到加入频道Notify回调(room.on('NotifyJoinRoom', (data) =&gt; {}))](#room接收到加入频道notify回调roomonnotifyjoinroom-data--)
               * [room接收到退出频道Notify回调(room.on('NotifyLeaveRoom', (data) =&gt; {}))](#room接收到退出频道notify回调roomonnotifyleaveroom-data--)
               * [room接收设置用户属性Notify的回调(room.on('NotifyUserAttributesSet', (data) =&gt; {}))](#room接收设置用户属性notify的回调roomonnotifyuserattributesset-data--)
               * [room接收到删除用户某些属性Notify回调(room.on('NotifyUserAttributesDelete', (data) =&gt; {}))](#room接收到删除用户某些属性notify回调roomonnotifyuserattributesdelete-data--)
               * [room接收到清除用户某些属性Notify回调(room.on('NotifyUserAttributesClear', (data) =&gt; {}))](#room接收到清除用户某些属性notify回调roomonnotifyuserattributesclear-data--)
               * [room接收到添加或更新用户某些属性Notify回调(room.on('NotifyUserAttributesAddOrUpdate', (data) =&gt; {}))](#room接收到添加或更新用户某些属性notify回调roomonnotifyuserattributesaddorupdate-data--)
               * [room接收设置频道属性Notify的回调(room.on('NotifyRoomAttributesSet', (data) =&gt; {}))](#room接收设置频道属性notify的回调roomonnotifyroomattributesset-data--)
               * [room接收到删除频道某些属性Notify回调(room.on('NotifyRoomAttributesDelete', (data) =&gt; {}))](#room接收到删除频道某些属性notify回调roomonnotifyroomattributesdelete-data--)
               * [room接收到清除频道某些属性Notify回调(room.on('NotifyRoomAttributesClear', (data) =&gt; {}))](#room接收到清除频道某些属性notify回调roomonnotifyroomattributesclear-data--)
               * [room接收到添加或更新频道某些属性Notify回调(room.on('NotifyRoomAttributesAddOrUpdate', (data) =&gt; {}))](#room接收到添加或更新频道某些属性notify回调roomonnotifyroomattributesaddorupdate-data--)
         * [【辅助工具】getInstanceInfo获取实例信息(getInstanceInfo)](#辅助工具getinstanceinfo获取实例信息getinstanceinfo)
         * [【辅助工具】将string编码成Utf8二进制(encodeStringToUtf8Bytes)](#辅助工具将string编码成utf8二进制encodestringtoutf8bytes)
         * [【辅助工具】将Utf8二进制解码成string类型(decodeUtf8BytesToString)](#辅助工具将utf8二进制解码成string类型decodeutf8bytestostring)


## js-sdk主要功能

	登录登出、更新Token
	点对点消息
	查询用户在线状态
	加入离开频道相关
	频道消息
	频道内用户属性增删改查
	频道属性增删改查
	查询单个或多个频道的成员人数
	获取频道成员列表


## js-sdk对外接口

### 注意事项

（1）本系统支持使用64位数字的用户ID(uid)。由于javascript的number类型不支持64位的整数。所以sdk使用string来存储uid。

（2）一个浏览器只能登录一个用户（uid）。

roomService区分可靠P2P；可靠组播；
    

（I）白板功能调试主要场景：（时延较小，满足需求）

    （a）【1对1白板授课】可靠的p2p；

    （b）【1对多白板授课】可靠的组播；

（II）另外一个场景是1对多，老师禁掉其他学生的声音的控制；采用可靠p2p


### 初始化Hummer

Hummer初始化：创建hummer实例

```javascript
    hummer = Hummer.createHummer({ appid });
```
入参:

| Name       | Type   | Description                                                                              |
| ---------- | ------ | ---------------------------------------------------------------------------------------- |
| appid      | number |  项目的appid                                                                             |

#### 登录(login)

```javascript
hummer.login({ uid, token });
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
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


#### 接收连接状态变更的回调通知(hummer.on('ConnectionStateChange', (data) => {}))

```javascript
hummer.on('ConnectionStateChange', (data) => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ConnectionStateChange" |
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


#### 设置用户归属地(setUserRegion)

```javascript
hummer.setUserRegion({ region });
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region    | string                  | 用户归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"）                      |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


### 初始化Room Service

初始化： 创建ChanelService实例

```javascript
client = hummer.createInstance();
```

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    | Object | 返回roomService实例             |


#### P2P的消息处理

##### 发送P2P的消息(sendMessageToUser)

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
| onlineUids | string[]  | 在线的uid列表      |

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
{"rescode":0,"msg":"ok","onlineUids":["999000"]}
```

#### 频道消息处理

##### 创建单个频道实例(createRoom)

创建单个频道实例

```js
room = client.createRoom({ region, roomId })
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    | Object | 返回room实例                |

【注】可以创建多个频道实例，并且可以在不同的业务归属region


##### 加入频道(joinRoom)
```js
room.joinRoom();
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
        room.joinRoom(params).then(res => {
          console.log("joinRoom res:", res);
        }).catch(err => {
        });
```

##### 退出频道(leaveRoom)

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
        room.leaveRoom(params).then(res => {
          console.log("leaveRoom res:", res);
        }).catch(err => {
        });
```


##### room发送组播消息(sendMessageToRoom)

```js
room.sendMessageToRoom({})
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
room.sendMessageToRoom(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```


##### room设置本地用户属性(setLocalUserAttributes)

```js
room.setLocalUserAttributes({})
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
    room.setLocalUserAttributes(params).then(res => {
      console.log("setLocalUserAttributes Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room删除本地用户某些属性(deleteLocalUserAttributesByKeys)

```js
room.deleteLocalUserAttributesByKeys({})
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
    room.deleteLocalUserAttributesByKeys({ keys }).then(res => {
      console.log("deleteLocalUserAttributesByKeys Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```

##### room添加或更新本地用户的属性(addOrUpdateLocalUserAttributes)

```js
room.addOrUpdateLocalUserAttributes({attributes})
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
    room.addOrUpdateLocalUserAttributes(params).then(res => {
      console.log("addOrUpdateLocalUserAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room清空本地用户的属性(clearLocalUserAttributes)

```js
room.clearLocalUserAttributes()
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
    room.clearLocalUserAttributes().then(res => {
      console.log("clearLocalUserAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### room获取频道用户列表(getRoomUserList)

```js
room.getRoomUserList()
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
        room.getRoomUserList().then(res => {
          console.log("getRoomUserList res:", res);
        }).catch(err => {
        });
```

响应：
```javascript
{"appid":1350626568,"roomId":"test_room","users":["123","555","233333","1356662","3300235422","3300235423","3300235499","3300235888","135666911222"],"rescode":0}
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

##### client全量设置某指定频道的属性(setRoomAttributes)

```js
client.setroomAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |
| attributes | {[k: string]: string} | 频道属性key-value键值对 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


示例：
```js
    let params = { region, roomId, attributes };
    room.setroomAttributes(params).then(res => {
      console.log("setroomAttributes Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### client删除某指定频道的指定属性(deleteRoomAttributesByKeys)

```js
client.deleteRoomAttributesByKeys({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |
| keys | string[] | 频道属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    client.deleteroomAttributesByKeys({ keys }).then(res => {
      console.log("deleteroomAttributesByKeys Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```

##### client更新频道属性(addOrUpdateRoomAttributes)

```js
client.addOrUpdateRoomAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |
| attributes | {[k: string]: string} | 频道属性key-value键值对 |

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


##### client清空某指定频道的属性(clearRoomAttributes)

```js
client.clearRoomAttributes()
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region?    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    client.clearRoomAttributes().then(res => {
      console.log("clearRoomAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### client查询某指定频道的全部属性(getRoomAttributes)

```js
client.getRoomAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region?    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |
| attributes | {[k: string]: string} | 频道属性key-value键值对 |

示例：
```js
    client.getRoomAttributes({region, roomId}).then(res => {
      console.log("getRoomAttributes res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### client查询某指定频道指定属性名的属性(getRoomAttributesByKeys)

```js
client.getRoomAttributesByKeys({regin, roomId})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| region?    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| roomId    | string                  | 频道ID                      |
| keys | string[] | 频道属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |
| attributes | {[k: string]: string} | 频道属性key-value键值对 |

示例：
```js
    client.getRoomAttributesByKeys({region, roomId, keys}).then(res => {
      console.log("getRoomAttributesByKeys res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### client查询单个或多个频道用户数(getRoomUserCount)

在预先设定的区域，查询单个或多个频道用户数

```js
client.getRoomUserCount({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region?    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
|    roomIds        |      string[]        |   同一区域的频道列表       |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    userCount       |      {[roomId:string]:number}     |     |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    client.getRoomUserCount({ region, roomIds }).then(res => {
      console.log("res:", res);
    }).catch(err => {
    });
```

响应示例：
```javascript
{"appid":1350626568,"userCount":{"test123":0,"test999":3},"rescode":0,"msg":""}
```


##### room接收组播消息(room.on('RoomMessage', (data) => {}))
接收指定频道的消息
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
| message | Message | 频道消息对象                |
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

##### room接收到加入频道Notify回调(room.on('NotifyJoinRoom', (data) => {}))

接收加入频道的用户列表通知。

```
room.on('NotifyJoinRoom', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyJoinRoom"    |
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

##### room接收到退出频道Notify回调(room.on('NotifyLeaveRoom', (data) => {}))

接收退出频道的用户列表通知。

```
room.on('NotifyLeaveRoom', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyLeaveRoom"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |

参考示例：

```javascript
{"uid":["135666911222"]}
```

##### room接收设置用户属性Notify的回调(room.on('NotifyUserAttributesSet', (data) => {}))

接收到该频道设置用户某些属性Notify回调通知。

```
room.on('NotifyUserAttributesSet', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesSet" |
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

##### room接收到删除用户某些属性Notify回调(room.on('NotifyUserAttributesDelete', (data) => {}))

接收到该频道删除用户某些属性Notify回调通知

```
room.on('NotifyUserAttributesDelete', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesDelete" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| lastUpdateTs     | string                  | 用户属性最近一次删除的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"999888","lastUpdateTs":"1585895009875","attributes":{"room_role_name":"student"}}
```

##### room接收到清除用户某些属性Notify回调(room.on('NotifyUserAttributesClear', (data) => {}))

接收到该频道清除用户某些属性Notify回调通知

```
room.on('NotifyUserAttributesClear', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesClear" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| lastUpdateTs     | string                  | 用户属性最近一次删除的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"999888","lastUpdateTs":"1585895009875","attributes":{"room_role_name":"student"}}
```

##### room接收到添加或更新用户某些属性Notify回调(room.on('NotifyUserAttributesAddOrUpdate', (data) => {}))

接收到添加或更新用户某些属性Notify回调通知

```
room.on('NotifyUserAttributesAddOrUpdate', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesAddOrUpdate" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |

data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| lastUpdateTs     | string                  | 用户属性最近一次更新的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 用户属性 |


参考示例：

```javascript
{"uid":"999888","lastUpdateTs":"1585894858984","attributes":{"Name":"awu","room_role_name":"student"}}
```


##### room接收设置频道属性Notify的回调(room.on('NotifyRoomAttributesSet', (data) => {}))

接收设置频道属性Notify的回调通知。

```
room.on('NotifyRoomAttributesSet', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyRoomAttributesSet" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |

data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更频道属性的用户UID |
| lastUpdateTs     | string                  | 频道属性最近一次设置的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 频道属性 |

参考示例：

```javascript
{"lastUpdateUid":"999888","lastUpdateTs":"1585893855556","attributes":{"Name":"awu","Bulletin":"bull","Extention":"ex","room_name":"nginx大讲堂"}}
```

##### room接收到删除频道某些属性Notify回调(room.on('NotifyRoomAttributesDelete', (data) => {}))

接收到该频道删除用户某些属性Notify回调通知

```
room.on('NotifyRoomAttributesDelete', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyRoomAttributesDelete" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更频道属性的用户UID |
| lastUpdateTs     | string                  | 频道属性最近一次删除的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 频道属性 |


##### room接收到清除频道某些属性Notify回调(room.on('NotifyRoomAttributesClear', (data) => {}))

接收到该频道清除用户某些属性Notify回调通知

```
room.on('NotifyRoomAttributesClear', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyRoomAttributesClear" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更频道属性的用户UID |
| lastUpdateTs     | string                  | 频道属性最近一次删除的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 频道属性 |




参考示例：

```javascript
{"lastUpdateUid":"999888","lastUpdateTs":"1585893904930","attributes":{"room_name":"nginx大讲堂"}}
```


##### room接收到添加或更新频道某些属性Notify回调(room.on('NotifyRoomAttributesAddOrUpdate', (data) => {}))

接收到添加或更新频道某些属性Notify回调通知

```
room.on('NotifyRoomAttributesAddOrUpdate', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyRoomAttributesAddOrUpdate" |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| lastUpdateUid    | string                  | 最近一次变更频道属性的用户UID |
| lastUpdateTs     | string                  | 频道属性最近一次更新的时间戳（毫秒） |
| attributes       | {[k:string]: string}    | 频道属性 |


参考示例：

```javascript
{"lastUpdateUid":"999888","lastUpdateTs":"1585893996807","attributes":{"room_name":"nginx大讲堂","owner":"awu"}}
```

### 【辅助工具】getInstanceInfo获取实例信息(getInstanceInfo)

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

### 【辅助工具】将string编码成Utf8二进制(encodeStringToUtf8Bytes)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
|      | string |             |

响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|          | Utf8Bytes |           |


示例：
```js
Hummer.Utify.encodeStringToUtf8Bytes(content);
```

### 【辅助工具】将Utf8二进制解码成string类型(decodeUtf8BytesToString)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
|      | Utf8Bytes |        |     |

响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|          | string |     |      |

示例：
```js
Hummer.Utify.decodeUtf8BytesToString(content);
```
