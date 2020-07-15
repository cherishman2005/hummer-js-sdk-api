# FAQ

## API使用注意事项

Hummer.createHummer()与hummer.login()分开操作；最好不要放在一起调用。

## login登录失败的问题排查
hummer.login登录 是根据业务的入参：appid, uid, token(otp)等参数给后端发了一个请求。

如果调试阶段，登录不成功。一般有如下情形：
* 业务调用hummer.login({})时入参填错；
* 后端升级，调整出现了bug。

排查思路：先在日志中拿到appid, uid, token(otp)等参数；然后找相关同事对。

## hummer token过期的处理

```javascript
hummer.on('TokenExpired'，() => {
  //业务调用hummer.refreshToken({ token });刷新token
});
```

## ChatRoomUserOffline 监听事件的回调处理

```javascript
chatroom.on('ChatRoomUserOffline'，() => {
  //需要业务捕捉到这个监听，网络恢复正常后调用hummer.joinChatRoom({});重新加入聊天室才能继续收这个聊天室的消息
});
```

## key-value限定条件
key值不要支持非可见字符（如中文）； key只需要支持可见字符
`[a-zA-Z0-9_-]`

# leaveChatRoom调用成功后，同一聊天室的其他用户没有收到该用户下线的通知

1. 首先 需要确认 leave 返回码rescode =0。 表示 sdk请求成功了。 —— sdk正常

1.  确认 同一聊天室的其他用户 监听逻辑是否正常？

1. 然后排查：

    （1）是否存在同一个UID多处登录；
        —— 如果多处登录，则收不到该uid下线通知的；（逻辑正常）

    （2）如果（1）确定后，没有多处登录，就需要拿case让后端分析。
    
# leave/join等所有聊天室请求API操作注意事项

* 必须在login调用返回ok（rescode=0）后才能进行这些API操作；
    如：业务在重新登录的时候，没有监听sdk login的回调成功，这时候连接还没有建立好，就发了leave，所以会超时。
