

# Hummer chatroom js-sdk

本文档只描述如下新增功能API:

* 消息通道管理_通道管理；

## js-sdk对外接口

### 注意事项

（1）本系统支持使用64位数字的用户ID(uid)。由于javascript的number类型不支持64位的整数。所以sdk使用string来存储uid。

### Message

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

【注】
* 测试无感知，只是组成一个json-object对象。


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


##### fetchUserOnlineStatus

批量查询用户在线状态

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
| onlineStatus | {[uid: string]: boolean} | 用户/在线状态键值对      |

示例：
```js
client.fetchUserOnlineStatus({uids: uids}).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```

响应：
```js
{"rescode":0,"msg":"ok","onlineStatus":{"999000":true}}
```

##### 发送点对点的消息(sendP2PMessage)

发送P2P的消息

```javascript
hummer.sendP2PMessage({})；
```

请求参数：

| Name      | Type                    | Description                                         |
| --------- | ----------------------- | --------------------------------------------------- |
| uid       | string                  | 接收者UID                       |
| message   | Message              | 消息的内容。                                        |
| options?  | any                  | 可选参数（暂不用暴露）                                       |


响应数据：Promise<>

| Name      | Type   | Description                 |
| --------- | ------ | --------------------------- |
| rescode   | number | 0：表示成功                 |
| msg       | string | 返回描述                    |

示例：
```javascript
let params = { uid, message, options }
hummer.sendP2PMessage(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err)
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```

##### hummer.on('P2PMessageReceived', (data) => {})

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


### channel

#### createChannelInstance

创建channel实例

```javascript
channel = hummer.createChannelInstance({region, channelId})
```

请求参数：


| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
| region    | string                  | chatroom归属地（"cn"/"ap_southeast"/"ap_south" / "us" / "me_east" / "sa_east"） |
| channelId             |      string      |   channel ID       |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|                       | object            | channel实例    |



#### joinChannel
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

#### leaveChannel

请求参数：

| Name | Type   | Description |
| ---- | ------ | ----------- |
| extra  | {[k: string]: string} | 可扩展字段（选填字段） |


响应数据：Promise<>

| Name     | Type    | Description  |
| -------- | ------- | ------------ |
|  rescode | number  |   0:成功      |
|  msg     | string  |   返回描述    |

示例：
```js
  let params = {extra};
  channel.leaveChannel(params).then(res => {
    console.log("leaveChannel res:", res);
  }).catch(err => {
  });
```


#### 发送广播消息(sendP2CMessage)

```js
channel.sendP2CMessage({})
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
room.sendP2CMessage(params).then(res => {
  console.log("res: " + JSON.stringify(res));
}).catch(err => {
  console.log(err);
})
```
响应：
```javascript
{"rescode":0,"msg":"ok"}
```

#### 接收广播消息(channel.on('P2CMessageReceived', (data) => {}))
接收指定房间的消息
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
| peerId  | string  | 发送者用户ID                 |
| message | Message | Message消息对象              |
