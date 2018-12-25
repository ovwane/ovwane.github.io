---
title: iOS WebDriverAgent 使用
date: 2018-12-15 13:55:07
tags:
---


wda 位置
```
~/.nvm/versions/node/v8.13.0/lib/node_modules/appium/node_modules/appium-xcuitest-driver/WebDriverAgent
```

获取状态信息

```
http://localhost:8100/status
```

获取source

```
http://localhost:8100/source

http://127.0.0.1:8100/source?format=json
```

获取session详细信息

```
http://localhost:8100/session/D2BDF992-D087-4013-B354-05F48FC5A748/
```

获取source

```
http://localhost:8100/session/D2BDF992-D087-4013-B354-05F48FC5A748/source
```

获取截图

```
http://localhost:8100/session/D2BDF992-D087-4013-B354-05F48FC5A748/screenshot

http://127.0.0.1:8100/screenshot
```

## FAQ

[JSONWP cannot find "wda/screen" ](https://github.com/appium/appium-desktop/issues/414#issuecomment-367238543)