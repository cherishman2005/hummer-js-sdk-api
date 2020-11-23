

<!-- TOC -->

- [Hummer chatroom js-sdk](#hummer-chatroom-js-sdk)
    - [chatroom历史消息获取](#chatroom历史消息获取)
    - [chatroom聊天室房间管理_房间扩展属性](#chatroom聊天室房间管理_房间扩展属性)
        - [房间属性操作API](#房间属性操作api)
            - [设置房间扩展属性](#设置房间扩展属性)
            - [更新房间扩展属性](#更新房间扩展属性)
            - [删除房间的指定扩展属性](#删除房间的指定扩展属性)
            - [清空某指定房间的扩展属性](#清空某指定房间的扩展属性)
        - [notify监听](#notify监听)
            - [接收设置房间属性Notify的回调](#接收设置房间属性notify的回调)
            - [接收到删除房间某些扩展属性Notify回调](#接收到删除房间某些扩展属性notify回调)
            - [接收到清除房间某些属性Notify回调](#接收到清除房间某些属性notify回调)
            - [接收到更新房间某些扩展属性Notify回调](#接收到更新房间某些扩展属性notify回调)
    - [消息通道](#消息通道)
        - [创建message对象](#创建message对象)
        - [P2P消息](#p2p消息)
            - [批量查询用户在线状态](#批量查询用户在线状态)
            - [发送点对点（P2P）的消息](#发送点对点p2p的消息)
            - [监听P2P消息](#监听p2p消息)
        - [P2C消息](#p2c消息)
            - [创建channel实例](#创建channel实例)
            - [加入channel](#加入channel)
            - [离开channel](#离开channel)
            - [发送channel（P2C）消息](#发送channelp2c消息)
            - [监听channel（P2C）消息](#监听channelp2c消息)

<!-- /TOC -->

# Hummer chatroom js-sdk

本文档只描述如下新增功能API:

* 拉取历史消息；

* 聊天室房间管理_房间扩展属性；

* 消息通道；


## chatroom历史消息获取

查询历史消息
```js
chatroom.fetchHistoryMessages({})
```

请求参数：

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

响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |
| hasMore  | bealean  |  是否还有消息，true: 有，false: 无    |
| messages | Message[]  |  消息列表    |

```
interface Message {
    msgType: MsgType;
    content: string;
    uuid: string;
    timestamp: number;
}
```


## chatroom聊天室房间管理_房间扩展属性

### 房间属性操作API

#### 设置房间扩展属性

```js
chatroom.setRoomExtraAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraAttributes | {[k: string]: string} | 房间属性key-value键值对 |


响应数据：Promise<>

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


#### 更新房间扩展属性

```js
chatroom.updateRoomExtraAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraAttributes | {[k: string]: string} | 房间属性key-value键值对 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
| rescode  | number  |   0:成功     |
| msg      | string  | 返回描述     |


示例：
```js
try {
    let params = { extraAttributes };
    const res = await chatroom.updateRoomExtraAttributes(params);
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

#### 删除房间的指定扩展属性

```js
chatroom.deleteRoomExtraAttributes({})
```

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extraKeys | string[] | 房间属性key数组 |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  | 返回描述      |

示例：
```js
try {
    const res = await chatroom.deleteRoomExtraAttributes({extraKeys});
    console.log("res=", res);
} catch (e) {
    console.log(e);
}
```

#### 清空某指定房间的扩展属性

```js
chatroom.clearRoomExtraAttributes()
```

请求参数：（无）


响应数据：Promise<>

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

### notify监听

#### 接收设置房间属性Notify的回调

接收设置房间属性Notify的回调通知。

```
chatroom.on('RoomExtraAttributesSet', (data) => { console.log(data); })
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

#### 接收到删除房间某些扩展属性Notify回调

接收到该房间删除用户某些属性Notify回调通知

```
chatroom.on('RoomExtraAttributesDeleted', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomExtraAttributesDeleted" |
| handler | function  | 接收回调                 |

handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |


data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |


#### 接收到清除房间某些属性Notify回调

接收到该房间清除用户某些属性Notify回调通知

```
chatroom.on('RoomExtraAttributesCleared', (data) => { console.log(data); });
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
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |



#### 接收到更新房间某些扩展属性Notify回调

接收到添加或更新房间某些属性Notify回调通知

```
chatroom.on('RoomExtraAttributesUpdated', (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"RoomExtraAttributesUpdated" |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object  | 详细定义将data定义描述      |



data定义：

| name             | type                    | description                                             |
| ---------------- | ----------------------- | ------------------------------------------------------- |
| uid           | string                  | 最近一次变更房间属性的用户UID |
| lasttimeTs    | number                  | 最近一次变更房间属性的时间戳 |
| extraAttributes       | {[k:string]: string}    | 房间扩展属性 |


## 消息通道

### 创建message对象

构造消息对象

| Name       | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| msgType    | MessageType(enum)     |  消息类型             |
| content    | string|Uint8Array     |  消息内容             |
| appExtra   | {[k: stirng]: string} |  扩展信息             |

接口定义如下：
```
enum MessageType {
    TextType  = 0,
    BinType   = 1,
    ImageType = 2,
};

interface Message {
    msgType: MessageType;
    content: string|Uint8Array;
    appExtra: {[k: stirng]: string}
};
```

示例：
```
let content = 'js-sdk message';
let message = Hummer.createMessage(0, content);
```

### P2P消息

#### 批量查询用户在线状态

```javascript
hummer.fetchUserOnlineStatus({ uids })
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
| status | {[uid: string]: OnlineStatus} | 用户/在线状态键值对      |

OnlineStatus定义：
```
enum OnlineStatus {
    UNDEFINED = -1,
    OFFLINE = 0,
    ONLINE = 1,
    DISCONNECTED = 2,
};
```

示例：
```js
try {
    const res = await this.hummer.fetchUserOnlineStatus({uids});
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.log(err);
}
```

#### 发送点对点（P2P）的消息

```javascript
hummer.sendP2PMessage({})；
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | -------------------------------------------- |
| recevier  | string                  | 接收者UID                                     |
| message   | Message              | 消息的内容。                                        |
| options  | {isOffline： boolean}  | 离线、在线选项                                      |


响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
try {
    let params = { uid, message, options }
    const res = await hummer.sendP2PMessage(params);
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.error(e);
}
```

#### 监听P2P消息

接收对端的P2P消息
```javascript
hummer.on(eventName: 'P2PMessageReceived', handler: (data) => { console.log(data); });
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"P2PMessageReceived"   |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
|         | object | 详细定义将data定义描述       |


data定义：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| peerId  | string   | 用户ID                      |
| message | Message  | 详见构造消息对象（Message）   |




### P2C消息

#### 创建channel实例

```javascript
channel = hummer.createChannel({region, channelId})
```

请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region    | string                  | channel归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| channelId             |      string      |   channel ID       |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|                       | object            | channel实例    |

#### 加入channel

```js
channel.joinChannel();
```

请求参数：（无）


响应数据：Promise<>

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

#### 离开channel

```js
channel.leaveChannel();
```

请求参数：（无）


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  |   返回描述    |

示例：
```js
try {
    const res = await channel.leaveChannel();
    console.log("leaveChannel res:", res);
} catch(e) {
    console.error(e);
}
```

#### 发送channel（P2C）消息

```js
channel.sendP2CMessage({})
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| type      | string                  | content的类型。用户自己约定。                       |
| message   | Message                 | Message消息对象                                    |
| appExtras | { [k: string]: string } | 可选参数。 用户自定义的数据。 键和值为string的Object。 |

响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
try {
    let params = { message }
    const res = await channel.sendP2CMessage(params);
    console.log("res: " + JSON.stringify(res));
} catch(e) {
    console.error(e);
}
```

#### 监听channel（P2C）消息

接收指定channel的消息
```javascript
channel.on('P2CMessageReceived', (data) => { console.log(data); })
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string | 取值"P2CMessageReceived"    |
| handler | function  | 接收回调                 |


handler回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| opUid   | string  | 发送者用户ID                 |
| message | Message | Message消息对象              |
