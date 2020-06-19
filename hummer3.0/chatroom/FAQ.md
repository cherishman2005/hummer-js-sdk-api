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
