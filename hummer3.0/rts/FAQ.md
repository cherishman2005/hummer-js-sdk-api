# FAQ

## API使用注意事项

Hummer.createHummer()与hummer.login()分开操作；最好不要放在一起调用。

## hummer token过期的处理

```javascript
hummer.on('TokenExpired'，() => {
  //业务调用hummer.refreshToken({ token });刷新token
});
```


