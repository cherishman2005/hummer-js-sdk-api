[TOC]
# Hummer IM js-sdk

## js-sdk对外接口
### 初始化Hummer
```javascript
const hummer = new Hummer.Hummer({ appid: [appid], 
                            uid: [uid],
                            token: [token],
                            token_type: [token_type],  // 可选字段
                            area: [area],  // 'CN'暂只支持中国区
                            onConnectStatus: onConnectStatus,
                            onLoginStatus: onLoginStatus,
                            onerror: onerror
                          });
```

入参:

| Name                  | Type              |  Description |
| ----------------------| --------------- | -------------- |
|    appid             |      number          | |
|    uid               |      string          | 64bit整型对应的字符串 |
|    token             |      string          | |
|    token_type        |      string          | token类型（可选字段，不填默认为"TOKEN_3RD"）, 只能取"TOKEN_3RD"，"YY_TOKEN"|
|    area              |      string          |'CN'表示中国区，其他表示海外 |
| onConnectStatus      |      Function        | 接收连接状态变更信息  |
| onLoginStatus        |      Function        | 接收登录状态变更消息  |
|    onerror           |      Function        | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"}                         |

### hummer刷新Token
请求参数：

| Name                  | Type              | Description |
| --------------------- | ----------------- | -------------- |
|    uid               |      string          |  64bit整型对应的字符串 |
|    token             |      string          | |

```javascript
hummer.refreshToken({uid, token}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
});
```

### 初始化IM
```javascript
const im = new Hummer.IM(hummer, {  
                                      onRecvIMPushMsg: onRecvIMPushMsg,
                                      onerror: onerror
                                      });
```

入参：

| name                     | type | description       |
| -------------------------| ---------- | ----------------- |
| onRecvIMPushMsg      | function |      接收IMPush消息           |
| onerror                  | Function | 返回json-object，如果code=0，表示成功。如{"code":0,"msg":"ok"} |


### 设置接收消息的回调
设置setChatroomRecvCb回调
setIMRecvCb({  onRecvIMPushMsg,
               onConnectStatus })

```javascript
im.setIMRecvCb({onRecvIMPushMsg: onRecvIMPushMsg})
```

### 发送请求消息的接口

#### GetInstance获取实例
请求参数：

| Name                  | Type              |
| ---------------------  | ----------------- |
|    NA           |                | |


响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    appid             |      number          |   |
|    uid               |      string          |   UID |
|    instanceId         |      number          |    实例ID   |

示例：
```javascript
        im.getInstance().then(res => {
          console.log("GetInstance: " + JSON.stringify(res));
        }).catch(err => {
          console.log(err);
        });
```
响应：
```javascript
{ "uid": "123", "instanceId": 3584889511, "isAnonymous": false, "appid": 1350626568 }
```


#### initPullMsg初始化拉取数据
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    mode               |     number        |  0: 以最新的记录开始拉取(current)；1: 以上次的记录开始拉取(last)；2: 全量拉取     |

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    Array              |  Array(object)    | object key值包括：from_uid，to_uid，msg_type，content，uuid，timestamp  |

示例：
```javascript
    im.initPullMsg({ mode }).then(res => {
      console.log("InitPullMsg: " + JSON.stringify(res));
    }).catch(err => {
    });
```
响应：
```javascript
[{
	"from_uid": "135666911222",
	"to_uid": "123",
	"msg_type": 0,
	"content": "具体",
	"uuid": "174175d8-f59c-46b2-afcd-b51645ada9c1",
	"timestamp": "1567848412580"
}, {
	"from_uid": "135666911222",
	"to_uid": "123",
	"msg_type": 0,
	"content": "旅途",
	"uuid": "3f00dc0d-32d1-45fe-ac7a-7ac448f3eab4",
	"timestamp": "1567848417939"
}]
```


#### sendP2PChat客户端给B用户发消息
请求参数：

| Name                  | Type              |  Description |
| --------------------- | ----------------- |   ----------------- |
|    content                    |     string           |          |
|    msg_type                   |     number           |  0: text; 1: image; 2: url; 10000:自定义（暂支持msg_type=0） |
|    receiver                   |     string           | 接收者uid |
|    appExtra                   |     string           | 业务自定义信息 |
|    onmessage                  |     Function           | 回调message object对象业务使用，如uuid等关键字 |

响应数据：

| Name                  | Type              |  Description |
| --------------------- | ----------------- | ------------  |
|    uuid             |      string          | 关键字uuid |
|    log_id             |      string          | 64bit整型对应的字符串 |
|    code               |      number          |   0：表示成功 |
|    msg                |      string          |    返回描述   |
|    timestamp         |      string          | 64bit整型对应的字符串 |

示例：
```javascript
    let params = { content, receiver, appExtra, onmessage }
    im.sendP2PChat(params).then((res) => {
      console.log("SendP2PChat Res: " + JSON.stringify(res));
    }).catch((err) => {
      console.log(err)
    })
```
响应示例：
```javascript
{"uuid":"e41f23a1-f63b-4ce8-b879-cc37d7ce8b28","log_id":"6742377795998973953","code":0,"msg":"ok","timestamp":"1569832161863"}
```
onmessage回调示例：
```javascript
{"uuid":"e41f23a1-f63b-4ce8-b879-cc37d7ce8b28","log_id":"6742377795998973953","from_uid":"123","to_uid":"135666911222","msg_type":0,"content":"js_sdk SendP2PChat"}
```

### 接收消息的接口
| 接收消息的Function         | description       |
| -------------------------- | ----------------- |
| onRecvIMPushMsg           | 接收IMPush消息        |

#### onRecvIMPushMsg接收对端IMPush消息
| Name                  | Type              |  Description |
| --------------------- | --------------- | ----------- |
|    from_uid           |      string          | 64bit整型对应的字符串 |
|    to_uid             |      string          | 64bit整型对应的字符串 |
|    msg_type           |      number          |  |
|    content            |      string          |  |
|    uuid               |      string          |  |
|    timestamp          |      string          | 64bit整型对应的字符串 |

示例：
```javascript
{
	"from_uid": "135666911222",
	"to_uid": "123",
	"msg_type": 0,
	"content": "啦啦啦啦",
	"uuid": "74fc309c-7ed5-48fd-9f5c-350011585556",
	"timestamp": "1567847229893"
}
```

## 调试示例demo

https://service-test.sunclouds.com/room/im-test

### im-demo配置

采用http(CDN)方式引用聊天室js_sdk
```javascript
<script charset="utf-8" src=" https://***.**.com/hummer-im-x.x.x.js"></script>
```
使用js-sdk的业务部署在后端服务器。

【注】js_sdk带上版本号发布（如“1.0.2”）。

nginx.conf
```javascript
	location /room {
		try_files $uri $uri/ /room/index.html;
		default_type text/html;
		alias /home/zhangbiwu/vue_projects/room/dist;
	}
```
