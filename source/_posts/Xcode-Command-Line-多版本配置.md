---
title: Xcode Command Line 多版本配置
date: 2019-06-11 12:47:57
tags:
---



安装 ruby

```
brew install ruby
```



设置环境变量

```
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="/usr/local/lib/ruby/gems/2.6.0/bin:$PATH"' >> ~/.zshrc
```

> For compilers to find ruby you may need to set:
>   export LDFLAGS="-L/usr/local/opt/ruby/lib"
>   export CPPFLAGS="-I/usr/local/opt/ruby/include"



安装 [xcode-install](https://github.com/xcpretty/xcode-install)

```shell
gem install xcode-install
```



查看版本

```shell
xcversion --version
```



安装 Xcode

```
xcversion list
```

```shell
xcversion install "9.4.1"
```



查看当前 Xcode 版本

```shell
xcode-select --print-path
```

```shell
ls /Applications | grep Xcode
```



切换 Xcode 版本

```
xcversion select 9.4.1
```

```shell
sudo xcode-select --switch /Applications/Xcode-9.4.1.app
```



## Command Line Tools

```
xcversion install-cli-tools
```



## Simulators

```
xcversion simulators
```



## 问题

### [/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk/usr/lib/libSystem.tbd:4:18: error: unknown enumerated scalar ](https://github.com/esnme/ultrajson/issues/332)

```shell
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```



```shell
env CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install stf -g
```

> ['utility' file not found](https://github.com/nodejs/node-gyp/issues/1564)



## 参考

[ Installing and Switching Xcode Versions from the Command Line ](https://littlebitesofcocoa.com/314-installing-and-switching-xcode-versions-from-the-command-line)