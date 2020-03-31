---
title: ChromeDriver 编译
date: 2020-03-20 14:06:34
tags:
---



## 准备

系统依赖

- A 64-bit Mac running 10.14+.

- [Xcode](https://developer.apple.com/xcode) 11+

- The OS X 10.15 SDK. Run
	```shell
	ls `xcode-select -p`/Platforms/MacOSX.platform/Developer/SDKs
	```

- Python 2.7.16

- Git 2.25.2



## 设置代理

### 设置终端代理

```
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7891
```



### 设置 boto 代理

```
export NO_AUTH_BOTO_CONFIG=~/.boto
```

```
[Boto]
proxy=127.0.0.1
proxy_port=7890
proxy_type=http
```



## 下载源码

### 下载 depot_tools 工具

新建目录

```shell
mkdir chromium_src && cd chromium_src
```



安装 depot_tools

```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```



设置系统环境变量

```shell
export PATH="$PATH:${PWD}/depot_tools"
```



### 获取源码

开启

```
git config --global core.precomposeUnicode true
```



新建目录

```
mkdir chromium && cd chromium
```



获取代码

```shell
fetch --nohooks --no-history chromium --target_os=darwin
```

>`fetch --nohooks chromium`，不要加`--no-history`。
>
>`fetch --nohooks --no-history chromium`
>
>`fetch --nohooks --no-history chromium --target_os=darwin`

使用 pyenv 切换 python3 和 python2 都提示 `Warning: Running gclient on Python 3`。



切换目录

```shell
cd src
```



检查分支

```shell
git fetch --tags
```



拉取分支并切换

```shell
git checkout -b xweb_66.0.3359.126 66.0.3359.126
```

> Chrome 与 chromedriver 映射关系 https://github.com/appium/appium-chromedriver/blob/master/lib/chromedriver.js
>
> 67 使用的 macOS10.12.sdk。https://github.com/phracker/MacOSX-SDKs/releases



同步分支数据

```shell
gclient sync --with_branch_heads --jobs 16
```

> `gclient runhooks` # 如果有提示 runhooks 成功，则不需执行。
>
> `gclient sync --force`
>
> 打印日志：gclient sync --verbose --verbose --verbose
>
> ```
> gclient sync --force --nohooks --with_branch_heads
> ```



如果上面`gclient sync` 步骤报错需要手动下载文件，先安装 gsutil: `pip install gsutil`

```shell
cd src

gsutil cp gs://chromium-gn/a14b089cbae9c29ecbc781686ada8babac8550af buildtools/mac/gn

chmod +x buildtools/mac/gn

gsutil cp gs://chromium-clang-format/0679b295e2ce2fce7919d1e8d003e497475f24a3 buildtools/mac/clang-format

gsutil cp gs://chromium-luci/ffb6a624bd14abdff34618fe97562b34350199f7 tools/luci-go/mac64/isolate

gsutil cp gs://chromium-nodejs/8.9.1/c52ee3605efb50ae391bdbe547fb385f39c5a7a9 third_party/node/mac/node-darwin-x64.tar.gz

gsutil cp gs://v8-wasm-fuzzer/f6b95b7dd8300efa84b6382f16cfcae4ec9fa108 v8/test/fuzzer/wasm_corpus.tar.gz

gsutil cp gs://chromium-nodejs/050c85d20f7cedd7f5c39533c1ba89dcdfa56a08 third_party/node/node_modules.tar.gz
```



生成 args.gn 文件

```
gn gen --args="is_debug=false is_component_build = false symbol_level = 0" out/Release
```



文件内容：out/Release/args.gn

```
# Release
is_debug = false
# 不构建许多 dylib 文件
is_component_build = false
# 不要符号表，不用在 gdb 里面调试。
symbol_level = 0
```




编译 chromedriver

```
ninja -C out/Release chromedriver
```



## 修改代码

webview

```
chromium/src/chrome/device_manager.cc

std::string pattern = base::StringPrintf("@webview_devtools_remote_.*%d", pid);
```



命令行入参

```
chromium/src/chrome/test/chromedriver/server/chromedriver_server.cc
```



## 参考

 [Checking out and building Chromium for Mac](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md) 

 [Mac本地编译chromedriver](https://whisperloli.github.io/2019/07/04/compile_chromedriver) 

 [本地编译 chromedriver 历程记录 · TesterHome](https://testerhome.com/topics/16226) 