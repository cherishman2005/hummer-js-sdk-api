# Hummer Channel Service js-sdk

   * [Hummer Channel Service js-sdk](#hummer-channel-service-js-sdk)
      * [js-sdk对外接口](#js-sdk对外接口)
         * [注意事项](#注意事项)
         * [初始化Hummer](#初始化hummer)
            * [接收链接状态的回调通知(hummer.on('ConnectStatus', (data) =&gt; {}))](#接收链接状态的回调通知hummeronconnectstatus-data--)
            * [接收登录状态的回调通知(hummer.on('LoginStatus', (data) =&gt; {}))](#接收登录状态的回调通知hummeronloginstatus-data--)
         * [初始化Channel Service](#初始化channel-service)
            * [P2P的消息处理](#p2p的消息处理)
               * [设置用户归属地](#设置用户归属地)
               * [发送P2P的消息(sendMessageToUser)](#发送p2p的消息sendmessagetouser)
               * [接收对端消息（client.on('MessageFromUser', (data) =&gt; {})）](#接收对端消息clientonmessagefromuser-data--)
               * [查询单个用户在线(queryOnlineStatusForUser)](#查询单个用户在线queryonlinestatusforuser)
               * [批量查询用户在线(queryUsersOnlineStatus)](#批量查询用户在线queryusersonlinestatus)
            * [频道消息处理](#频道消息处理)
               * [创建单个频道实例(createChannel)](#创建单个频道实例createchannel)
               * [加入频道(joinChannel)](#加入频道joinchannel)
               * [退出频道(leaveChannel)](#退出频道leavechannel)
               * [channel发送组播消息(sendMessageToChannel)](#channel发送组播消息sendmessagetochannel)
               * [channel设置用户属性(setUserAttributes)](#channel设置用户属性setuserattributes)
               * [channel删除用户某些属性(deleteUserAttributesByKeys)](#channel删除用户某些属性deleteuserattributesbykeys)
               * [channel获取频道用户列表(getChannelUserList)](#channel获取频道用户列表getchanneluserlist)
               * [channel根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)](#channel根据用户某一属性获取频道用户列表getchanneluserlistbyatrribute)
               * [channel查询单个或多个频道用户数(getChannelUserCount)](#channel查询单个或多个频道用户数getchannelusercount)
               * [channel接收组播消息(channel.on('ChannelMessage', (data) =&gt; {}))](#channel接收组播消息channelonchannelmessage-data--)
               * [channel接收到加入频道Notify回调(channel.on('NotifyJoinChannel', (data) =&gt; {}))](#channel接收到加入频道notify回调channelonnotifyjoinchannel-data--)
               * [channel接收到退出频道Notify回调(channel.on('NotifyLeaveChannel', (data) =&gt; {}))](#channel接收到退出频道notify回调channelonnotifyleavechannel-data--)
               * [channel接收设置用户属性Notify的回调(channel.on('NotifyUserAttributesSet', (data) =&gt; {}))](#channel接收设置用户属性notify的回调channelonnotifyuserattributesset-data--)
               * [channel接收到删除用户某一属性Notify回调(channel.on('NotifyUserAttributesDelete', (data) =&gt; {}))](#channel接收到删除用户某一属性notify回调channelonnotifyuserattributesdelete-data--)
         * [getInstanceInfo获取实例信息(getInstanceInfo)](#getinstanceinfo获取实例信息getinstanceinfo)
         * [【辅助工具】将string编码成Utf8二进制(encodeStringToUtf8Bytes)](#辅助工具将string编码成utf8二进制encodestringtoutf8bytes)
         * [【辅助工具】将Utf8二进制解码成string类型(decodeUtf8BytesToString)](#辅助工具将utf8二进制解码成string类型decodeutf8bytestostring)


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

Hummer初始化：创建hummer实例

```javascript
      this.hummer = Hummer.createHummer({ appid: this.appid,
                                  uid: this.uid,
                                  token: this.token,
                                  onError: onError
                                });
```
入参:

| Name       | Type   | Description                                                                              |
| ---------- | ------ | ---------------------------------------------------------------------------------------- |
| appid      | number |                                                                                          |
| uid        | string | 64bit整型对应的字符串                                                                    |
| token      | string |                                                                                          |
| onError    | Function | 返回Object，如果code=0，表示成功。如{"code":0,"msg":"ok"}                         |


hummer初始化时，通过onError回调返回来回馈初始化是否成功。


#### 接收链接状态的回调通知(hummer.on('ConnectStatus', (data) => {}))

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ConnectStatus" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| code | number | enum ConnectStatus：链接状态 |
| msg | string  | 提示信息                |

```javascript
enum ConnectStatus {
    Disconnected = 0,
    Connecting = 1,
    Connected = 2,
}
```


#### 接收登录状态的回调通知(hummer.on('LoginStatus', (data) => {}))

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"LoginStatus" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| code | number | enum LoginStatus：登录状态 |
| msg | string  | 提示信息                |

```javascript
export enum LoginStatus {
    Idle = -1,
    Logined = 0,
}
```


### 初始化Channel Service

初始化： 创建ChanelService实例

```javascript
client = hummer.createInstance();
```

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    | Object | 返回ChannelService实例             |


#### P2P的消息处理

##### 设置用户归属地

client.setUserRegion({ region });

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region    | string                  | 用户归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"）                      |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |


##### 发送P2P的消息(sendMessageToUser)
发送P2P的消息
client.sendMessageToUser({});

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array              | 消息的内容。                                        |
| receiver  | string                  | 接收者uid                                           |
| option    | object                  | 可选设置项。现包括成员reliable，默认"yes"可靠；取值['yes', 'no']|
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的json-object。 |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { type, content, receiver, option: {reliable} }
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

##### 接收对端消息（client.on('MessageFromUser', (data) => {})）

```javascript
client.on('MessageFromUser', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesDelete" |
| handler | function  | 接收回调                 |

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


```typescript
interface Message {
    message: {
        type: string;
        data: Uint8Array;
        appExtras?: ({ [k: string]: string }|null);
        serverAcceptedTs: string;
    },
    fromUid: string;
}
```

##### 查询单个用户在线(queryOnlineStatusForUser)
查询单个用户在线
```javascript
client.queryOnlineStatusForUser({uid: uid});
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| uid  | string | 要查的uid   |

响应数据：Promise<>

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

##### 批量查询用户在线(queryUsersOnlineStatus)

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

【注】queryUsersOnlineStatus可以逐步替掉接口queryOnlineStatusForUser

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

##### 创建单个频道实例(createChannel)

创建单个频道实例

```js
channel = client.createChannel({ region, channelId })
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| region    | string                  | 业务归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| channelId    | string                  | 频道ID                      |

响应数据：

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
|    | Object | 返回Channel实例                |

【注】可以创建多个频道实例，并且可以在不同的业务归属region


##### 加入频道(joinChannel)
```js
channel.joinChannel();
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
        channel.joinChannel(params).then(res => {
          console.log("joinChannel res:", res);
        }).catch(err => {
        });
```

##### 退出频道(leaveChannel)

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
        channel.leaveChannel(params).then(res => {
          console.log("leaveChannel res:", res);
        }).catch(err => {
        });
```


##### channel发送组播消息(sendMessageToChannel)

```js
channel.sendMessageToChannel({})
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| content   | Uint8Array                  | 消息的内容。                                    |
| option    | object                  | 可选设置项。现包括成员reliable，默认"yes"可靠；取值('yes' | 'no')|
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的Object。 |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { type, content, option: {reliable} }
channel.sendMessageToChannel(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```


##### channel设置用户属性(setUserAttributes)

```js
channel.setUserAttributes({})
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
    let params = { attributes };
    channel.setUserAttributes(params).then(res => {
      console.log("setUserAttributes Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```


##### channel删除用户某些属性(deleteUserAttributesByKeys)

```js
channel.deleteUserAttributesByKeys({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| keys | string[] | 用户属性key数组 |


响应数据：

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功     |
| msg       | string | 返回描述     |

示例：
```js
    channel.deleteUserAttributesByKeys({ keys }).then(res => {
      console.log("deleteUserAttributesByKeys Res: ", res);
    }).catch(err => {
      console.log(err)
    })
```

##### channel获取频道用户列表(getChannelUserList)

```js
channel.getChannelUserList()
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    NA        |      NA        |     | |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    channelId       |      string        |     |
|    users             |      string[]        | 用户列表 |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
        this.channel.getChannelUserList().then(res => {
          console.log("getChannelUserList res:", res);
        }).catch(err => {
        });
```

响应：
```javascript
{"appid":1350626568,"channelId":"test_channel","users":["123","555","233333","1356662","3300235422","3300235423","3300235499","3300235888","135666911222"],"rescode":0}
```

##### channel根据用户某一属性获取频道用户列表(getChannelUserListByAtrribute)

```js
channel.getChannelUserListByAtrribute({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    key        |      string        |   用户属性key       |
|    prop        |      string        |  用户属性value值   |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    channelId       |      string        |     |
|    users             |      string[]          | 用户列表 |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    channel.getChannelUserListByAtrribute({ key, prop }).then(res => {
      console.log("getChannelUserListByAtrribute res:", res);
    }).catch(err => {
    });
```


##### channel查询单个或多个频道用户数(getChannelUserCount)

在预先设定的区域，channel查询单个或多个频道用户数

```js
channel.getChannelUserCount({})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    channelIds        |      string[]        |   同一区域的频道列表       |

响应数据：Promise<>

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ----------- |
|    appid             |      number        |     |
|    channelIdCount       |      {[channelId:string]:number}     |     |
|    rescode             |      number          | 0：表示成功|
| msg       | string | 返回描述     |

示例：
```javascript
    channel.getChannelUserCount({ channelIds: channelIds }).then(res => {
      console.log("res:", res);
    }).catch(err => {
    });
```

响应示例：
```javascript
{"appid":1350626568,"channelIdCount":{"test123":0,"test999":3},"rescode":0,"msg":""}
```



##### channel接收组播消息(channel.on('ChannelMessage', (data) => {}))
接收指定频道的消息
```javascript
channel.on('ChannelMessage', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"ChannelMessage" |
| handler | function  | 接收回调                 |


handler回调参数：

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

```typescript
interface ChannelMessage {
    message: {
        type: string;
        data: Uint8Array;
        appExtras?: ({ [k: string]: string }|null);
        serverAcceptedTs: string;
    },
    fromUid: string;
}
```

##### channel接收到加入频道Notify回调(channel.on('NotifyJoinChannel', (data) => {}))

```
channel.on('NotifyJoinChannel', (data) => { console.log(data); })
```
接收加入该频道的用户列表通知

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyJoinChannel" |
| handler | function  | 接收回调                 |


回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| handler  | object  | 详细定义后面描述            |


handler data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |

```javascript
{"uid":["555"]}
```

##### channel接收到退出频道Notify回调(channel.on('NotifyLeaveChannel', (data) => {}))

接收退出该频道的用户列表通知

```
channel.on('NotifyLeaveChannel', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyLeaveChannel" |
| handler | function  | 接收回调                 |


handler data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uids             | string[]                | UID数组                                                 |

Typescript定义参考：

```javascript
{"uid":["135666911222"]}
```

##### channel接收设置用户属性Notify的回调(channel.on('NotifyUserAttributesSet', (data) => {}))

接收到该频道设置用户某一属性Notify回调通知

```
channel.on('NotifyUserAttributesSet', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesSet" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| handler  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes       | object                  | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"123","attributes":{"Name":"阿武","Description":"js_sdk测试","Bulletin":"bull","Extention":"ex"}}
```

##### channel接收到删除用户某一属性Notify回调(channel.on('NotifyUserAttributesDelete', (data) => {}))

接收到该频道删除用户某一属性Notify回调通知

```
channel.on('NotifyUserAttributesDelete', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"NotifyUserAttributesDelete" |
| handler | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| notify  | object  | 详细定义后面描述            |


Message定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid             | string                   | UID                                                     |
| attributes      | object                   | 用户属性 |

Typescript定义参考：

```javascript
{"uid":"999000","attributes":{"channel_role_name":"teacher"}
```

### getInstanceInfo获取实例信息(getInstanceInfo)

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
