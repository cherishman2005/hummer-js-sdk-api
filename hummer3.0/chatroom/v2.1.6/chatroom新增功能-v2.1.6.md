

<!-- TOC -->

- [Hummer chatroom js-sdk](#hummer-chatroom-js-sdk)
    - [refreshToken1](#refreshtoken1)
    - [接收token即将过期的回调通知TokenWillExpire](#接收token即将过期的回调通知tokenwillexpire)
    - [getState](#getstate)

<!-- /TOC -->

# Hummer chatroom js-sdk

1. Token过期&更新机制
   * 新增刷新token的API：refreshToken1；废弃API: refreshToken；
   * 新增监听token即将过期接口: hummer.on("TokenWillExpire", () => {});

1. 多端在线与登录互踢机制

1. 获取连接所处状态
   * 新增API：getConnectionState；废弃API：getState

1. 连接状态变化回调优化

1. sdk内部优化


## refreshToken1

更新当前Token

```javascript
hummer.refreshToken1({ token });
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


## 接收token即将过期的回调通知TokenWillExpire

接收token即将过期的回调通知

```javascript
hummer.on('TokenWillExpire', () => {});
```

回调通知：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| eventName | string    | 取值"TokenWillExpire"       |
| handler   | function  | 接收回调                 |

回调参数：

| name    | type    | description                 |
| ------- | ------- | --------------------------- |
| NA      |         |  只用监听事件，data为空       |

【注】
* 接收token即将过期的回调通知。


## getState

获取SDK当前状态

为了保持版本兼容，getState暂时保留；

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



