# Hummer IM js-sdk

https://github.com/cherishman2005/hummer-js-sdk-api/

## 动态


## FAQ

### 动态

[2019-12-11] 提供寻欢IM P2PChat hummer-im js-sdk1.0.4版本

	接口不变，js-sdk内部优化升级

[2019-11-19] 提供寻欢IM P2PChat hummer-im-1.0.2.js联调版本

	接口不变，js-sdk内部优化升级

[2019-11-05] 提供寻欢IM P2PChat hummer-im-1.0.1.js联调版本

	接口不变，js-sdk内部优化

[2019-10-08] 提供寻欢IM P2PChat hummer-im-1.0.0.js联调版本

	（1）将IM的connect/login回调调整到hummer（core）模块来处理。详见《im-README对外接口》
	（2）函数命名调整： InitPullMsg改为 initPullMsg;  SendP2PChat改为sendP2PMessage; GetInstance改为getInstance;

[2019-09-18] 提供寻欢IM P2PChat hummer-im-1.0.0.js联调版本

完善接口说明:（详见《im-README对外接口.md》）

	（1）新增初始化回调onerror； —— 方便排查初始化问题，特别是联调阶段的初始化配置；
	（2）area和env参数合并，直接采用area；（'CN': 生产环境；'CN-TEST'：测试环境）
	（3）SendP2PChat的回调接口名msgCb改为onmessage；

[2019-09-07] 提供寻欢IM P2PChat js-sdk 0.9.0调试版本


### 场景说明
	【业务需要考虑】IM在上次关闭，到本次打开时有很多离线消息（如成千上万条）需要拉取的场景， 业务准备怎么处理？会不会影响用户体验。