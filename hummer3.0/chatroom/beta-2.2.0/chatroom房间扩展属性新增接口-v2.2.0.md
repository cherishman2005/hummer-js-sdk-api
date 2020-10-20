


# Hummer chatroom js-sdk

本文档只描述如下新增功能API:

* 聊天室房间管理_房间扩展属性；


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


### chatroom

##### 接收设置房间属性Notify的回调(room.on('RoomAttributesSet', (data) => {}))

接收设置房间属性Notify的回调通知。

```
chatroom.on('RoomAttributesSet', (data) => { console.log(data); })
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


##### 接收到更新房间某些属性Notify回调(room.on('RoomExtraAttributesUpdated', (data) => {}))

接收到添加或更新房间某些属性Notify回调通知

```
room.on('RoomExtraAttributesUpdated', (data) => { console.log(data); });
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
| roomExtraAttribute    | {[k: string]: string}                  | 房间属性名列表 |
| operator              | string    |   操作者的用户ID |
| updateTs              | number    |   房间属性最近一次更新的时间戳（毫秒） |





