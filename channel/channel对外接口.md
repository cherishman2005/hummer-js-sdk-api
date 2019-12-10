# Hummer Channel Service js-sdk

- [Hummer Channel Service js-sdk](#hummer-channel-js-sdk)
  - [js-sdk对外接口](#js-sdk%e5%af%b9%e5%a4%96%e6%8e%a5%e5%8f%a3)
    - [注意事项](#%e6%b3%a8%e6%84%8f%e4%ba%8b%e9%a1%b9)
    - [初始化Hummer](#%e5%88%9d%e5%a7%8b%e5%8c%96hummer)
    - [hummer刷新Token](#hummer%e5%88%b7%e6%96%b0token)
    - [初始化Channel Service](#%e5%88%9d%e5%a7%8b%e5%8c%96signal-service)
      - [接收消息回调函数(onReceiveMessage)](#%e6%8e%a5%e6%94%b6%e6%b6%88%e6%81%af%e5%9b%9e%e8%b0%83%e5%87%bd%e6%95%b0onreceivemessage)
      - [接收channel组播消息回调函数(onReceiveChannelMessage)]
      - [接收到上线Notify回调(onNotifyJoinChannel)]
      - [接收到下线Notify回调(onNotifyLeaveChannel)]
      - [设置用户属性Notify回调(onNotifyUserAttributesSet)]
      - [删除用户某一属性Notify回调(onNotifyUserAttributesDelete)]
    - [getInstance获取实例(getInstance)](#getinstance%e8%8e%b7%e5%8f%96%e5%ae%9e%e4%be%8bgetinstance)
    - [给用户发消息(sendMessageToUser)](#%e7%bb%99%e7%94%a8%e6%88%b7%e5%8f%91%e6%b6%88%e6%81%afsendmessagetouser)
    - [给指定channel发送组播消息(sendMessageToChannel)]
    - [查询用户在线状态(queryOnlineStatusForUser)](#%e6%9f%a5%e8%af%a2%e7%94%a8%e6%88%b7%e5%9c%a8%e7%ba%bf%e7%8a%b6%e6%80%81queryonlinestatusforuser)
    - [进入频道(joinChannel)]
    - [离开频道(leaveChannel)]
    - [设置用户属性(setUserAttributes)]
    - [删除用户某些属性(deleteUserAttributesByKeys)]
    - [获取频道用户列表(getChannelUserList)]
    - [根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)]
    - [辅助工具]encodeStringToUtf8Bytes和decodeUtf8BytesToString
## js-sdk对外接口

### 注意事项

（1）本系统支持使用64位数字的用户ID(uid)。由于javascript的number类型不支持64位的整数。所以sdk使用string来存储uid。

（2）一个浏览器只能登录一个用户（uid）。

ChannelService与SignalService互通兼容，新业务使用ChannelService。


channel区分可靠P2P、非可靠P2P；可靠组播、非可靠组播；
    
【注】非可靠：这里非可靠是在网络异常，或抖动时才会出现掉包；——非可靠可以满足绝大部分场景；
    
    对于高并发，时延要求高的场景建议采用非可靠设置（如白板）； 对于可靠性要求高的场景采用可靠设置；
    
（I）白板功能调试主要场景：（时延较小，满足需求）

    （a）【1对1白板授课】非可靠的p2p；

    （b）【1对多白板授课】非可靠的组播；

（II）另外一个场景是1对多，老师禁掉其他学生的声音的控制；采用可靠p2p


### 初始化Hummer
```javascript
const hummer = new Hummer.Hummer({ appid: [appid],
                            uid: [uid],
                            token: [token],
                            area: [area],  // 目前支持"CN"区
                            onConnectStatus: onConnectStatus,
                            onLoginStatus: onLoginStatus,
                            onerror: onerror
                          });
```
入参:

| Name       | Type   | Description                                                                              |
| ---------- | ------ | ---------------------------------------------------------------------------------------- |
| appid      | number |                                                                                          |
| uid        | string | 64bit整型对应的字符串                                                                    |
| token      | string |                                                                                          |
| area       | string | 'CN'表示中国区，其他表示海外                                                             |
| onConnectStatus      |      Function        | 接收连接状态变更信息  |
| onLoginStatus        |      Function        | 接收登录状态变更消息  |
| onerror    | Function | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"}                         |

### hummer刷新Token

请求参数：

| Name  | Type   | Description           |
| ----- | ------ | --------------------- |
| uid   | string | 64bit整型对应的字符串 |
| token | string |            |          |


示例：
```javascript
hummer.refreshToken({uid, token}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
});
```


### 初始化Channel Service

```javascript
channel = new Hummer.ChannelService(hummer, {
    onReceiveChannelMessage: onReceiveChannelMessage,
    onNotifyJoinChannel: onNotifyJoinChannel,
    onNotifyLeaveChannel: onNotifyLeaveChannel,
    onNotifyUserAttributesSet: onNotifyUserAttributesSet,
    onNotifyUserAttributesDelete: onNotifyUserAttributesDelete,
    onReceiveMessage: messageCallback,
    onerror: onerror
});
```

入参：

| name                  | type     | description                          |
| --------------------- | -------- | ------------------------------------ |
| onReceiveMessage      | Function | 接收到消息通知回调, 详见后面描述。   |
| onReceiveChannelMessage | Function | 接收到channel组播消息的回调, 详见后面描述。 |
| onNotifyJoinChannel   | Function | 接收到上线Notify回调, 详见后面描述。   |
| onNotifyLeaveChannel  | Function | 接收到下线Notify回调, 详见后面描述。   |
| onNotifyUserAttributesSet   | Function | 接收设置用户属性Notify的回调, 详见后面描述。   |
| onNotifyUserAttributesDelete  | Function | 接收到删除用户某一属性Notify回调, 详见后面描述。   |
| onerror               | Function | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"} |


#### 接收消息回调函数(onReceiveMessage)


回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| message | Message | 消息实体。 详细定义后面描述 |
| fromUid | string  | 发送者的uid                 |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| type             | number                  | 消息内容类型                                            |
| data             | Uint8Array              | 消息内容                                                |
| appExtras        | {[k: string]: string}   | 可选参数。 用户自定义的数据。 键和值为string的json-object。  |
| serverAcceptedTs | string                  | 服务器接收到消息的时间戳(毫秒)。64bit整型对应的字符串。 |


Typescript定义参考：

```typescript
interface Message {
    type: number;
    data: Uint8Array;
    appExtras?: ({ [k: string]: string } | null);
    serverAcceptedTs: string;
}

type MessageCallbackFnType = (message: Message, fromUid: string) => void;
```

#### 接收channel组播消息回调函数(onReceiveChannelMessage)


回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| message | Message | 消息实体。 详细定义后面描述 |
| fromUid | string  | 发送者的uid                 |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| type             | number                  | 消息内容类型                                            |
| data             | Uint8Array              | 消息内容                                                |
| appExtras        | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。  |
| serverAcceptedTs | string                  | 服务器接收到消息的时间戳(毫秒)。64bit整型对应的字符串。 |
| channelId      | string                  | 频道名                                                  |


#### 接收到上线Notify回调(onNotifyJoinChannel)

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| notify  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |
| channelId     | string                   | 频道名                                                  |



Typescript定义参考：

```javascript
{"uid":"555","channelId":"test_channel"}
```

#### 接收到下线Notify回调(onNotifyLeaveChannel)

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| notify  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |
| channelId     | string                   | 频道名                                                  |

Typescript定义参考：

```javascript
{"uid":"135666911222","channelId":"test_channel"}
```

#### 接收设置用户属性Notify的回调(onNotifyUserAttributesSet)

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| notify  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| channelId       | string                   | 频道名                                                  |
| attributes       | object                  | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"123","channelId":"test_channel1","attributes":{"Name":"阿武","Description":"js_sdk测试","Bulletin":"bull","Extention":"ex"}}
```

#### 接收到删除用户某一属性Notify回调(onNotifyUserAttributesDelete)

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| notify  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| channelId       | string                   | 频道名                                                  |
| attributes      | object                   | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"123","channelId":"test_channel1","attributes":{"Name":""}}
```

### getInstance获取实例(getInstance)
请求参数：

| Name | Type |
| ---- | ---- |
| NA   |      |


响应数据：

| Name       | Type   | Description |
| ---------- | ------ | ----------- |
| appid      | number |             |
| uid        | string | UID         |
| channelList | Array(string) | UID所在的频道列表 |
| instanceId | number | 实例ID      |

示例：
```javascript
channel.getInstance().then(res => {
  console.log("GetInstance: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
});
```
响应：
```javascript
{ "uid": "123", "instanceId": 3114848827, "channelList": [ "test_channel", "test" ], "isAnonymous": false, "appid": 1350626568 }
```


### 给用户发P2P消息(sendMessageToUser)
请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array              | 消息的内容。                                        |
| receiver  | string                  | 接收者uid                                           |
| option    | object                  | 可选设置项。现包括成员reliable，默认"yes"可靠；取值['yes', 'no']|
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。 |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { type, content, receiver, option: {reliable} }
channel.sendMessageToUser(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err)
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```

### 给指定channel发送组播消息(sendMessageToChannel)
请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array                  | 消息的内容。                                    |
| channelId | string                | 频道名                                              |
| option    | object                  | 可选设置项。现包括成员reliable，默认"yes"可靠；取值['yes', 'no']|
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。 |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |
| timestamp | string | 64bit整型对应的字符串(毫秒) |

示例：
```javascript
let params = { type, content, channelId, option: {reliable} }
channel.sendMessageToChannel(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err)
})
```
响应：
```javascript
{"rescode":0,"msg":"ok","timestamp":"1569328870441"}
```


### 查询用户在线状态(queryOnlineStatusForUser)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| uid  | string | 要查的uid   |

响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  | 0：表示成功  |
| msg      | string  | 返回描述     |
| uid      | string  | 用户uid      |
| isOnline | boolean | 用户是否在线 |


示例：
```js
channel.queryOnlineStatusForUser({uid: uid}).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```

响应：
```js
{"rescode":0, "msg":"ok", "uid":"123", "isOnline":true}
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
Hummer.Utify.encodeStringToUtf8Bytes(content)
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
Hummer.Utify.decodeUtf8BytesToString(content)
```


### 进入频道joinChannel

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| channelId  | string | 频道名（必填字段） |
| extra  | {[k: string]: string} | 可扩展字段（选填字段） |



响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
|  msg     | string  |   返回描述   |

示例：
```js
        let params = {channelId};
        this.signal.joinChannel(params).then(res => {
          console.log("joinChannel res:", res);
        }).catch(err => {
        });
```

### 离开频道leaveChannel

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| channelId  | string | 频道名（必填字段） |
| extra  | {[k: string]: string} | 可扩展字段（选填字段） |


响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
|  msg     | string  |   返回描述   |

示例：
```js
        let params = {channelId, extra};
        this.signal.leaveChannel(params).then(res => {
          console.log("leaveChannel res:", res);
        }).catch(err => {
        });
```

### 设置用户属性(setUserAttributes)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| channelId  | string | 频道名（必填字段） |
| attributes | {[k: string]: string} | 用户属性 |


响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |


示例：
```js
    let params = { channelId, attributes };
    this.channel.setUserAttributes(params).then((res) => {
      console.log("setUserAttributes Res: ", res);
    }).catch((err) => {
      console.log(err)
    })
```

### 删除用户某些属性(deleteUserAttributesByKeys)

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| channelId  | string | 频道名（必填字段） |
| keys | Array(string) | 用户属性key数组 |


响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   200:成功     |


示例：
```js
    this.channel.deleteUserAttributesByKeys({ channelId, keys }).then((res) => {
      console.log("deleteUserAttributesByKeys Res: ", res);
    }).catch((err) => {
      console.log(err)
    })
```

### 获取频道用户列表(getChannelUserList)
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    channelId        |      string        |     |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    channelId       |      string        |     |
|    users             |      Array(string)          | 用户列表 |
|    rescode             |      number          | 0：表示成功|

示例：
```javascript
        this.signal.getChannelUserList({channelId}).then(res => {
          console.log("getChannelUserList res:", res);
        }).catch(err => {
        });
```

响应：
```javascript
{"appid":1350626568,"channelId":"test_channel","users":["123","555","233333","1356662","3300235422","3300235423","3300235499","3300235888","135666911222"],"rescode":0}
```

### 根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    channelId        |      string        |     |
|    key        |      string        |   用户属性key       |
|    prop        |      string        |  用户属性value值   |

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    channelId       |      string        |     |
|    users             |      Array(string)          | 用户列表 |
|    rescode             |      number          | 0：表示成功|

示例：
```javascript
    this.channel.getChannelUserListByAtrribute({ channelId, key, prop }).then(res => {
      console.log("getChannelUserListByAtrribute res:", res);
    }).catch(err => {
    });
```

响应：
```javascript
{"appid":1350626568,"channelId":"test_channel","users":["123","555","233333","1356662","3300235422","3300235423","3300235499","3300235888","135666911222"],"rescode":0}
```
