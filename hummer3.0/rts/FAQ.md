# FAQ

## API使用注意事项

Hummer.createHummer()与hummer.login()分开操作；最好不要放在一起调用。

## hummer token过期的处理

```javascript
hummer.on('TokenExpired'，() => {
  //业务调用hummer.refreshToken({ token });刷新token
});
```

## key-value限定条件
key值不要支持非可见字符（如中文）； key只需要支持可见字符
`[a-zA-Z0-9_-]`
