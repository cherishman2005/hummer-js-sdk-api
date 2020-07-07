# FAQ

## API使用注意事项

Hummer.createHummer()与hummer.login()分开操作；最好不要放在一起调用。

## hummer token过期的处理

```javascript
hummer.on('TokenExpired'，() => {
  //业务调用hummer.refreshToken({ token });刷新token
});
```

## RoomMemberOffline 监听事件的回调处理

```javascript
room.on('RoomMemberOffline'，() => {
  //需要业务捕捉到这个监听，网络恢复正常后调用hummer.join({});重新加入房间才能继续收这个房间的消息
});
```

## key-value限定条件
key值不要支持非可见字符（如中文）； key只需要支持可见字符
`[a-zA-Z0-9_-]`

# leave调用成功后，同一房间的其他用户没有收到该用户下线的通知

1. 首先 需要确认 leave 返回码rescode =0。 表示 sdk请求成功了。—— sdk正常

1.  确认 同一房间的其他用户 监听逻辑是否正常？

1. 然后排查：

    （1）是否存在同一个UID多处登录；
        —— 如果多处登录，则收不到该uid下线通知的；（逻辑正常）

    （2）如果（1）确定后，没有多处登录，就需要拿case让后端分析。