---
title: macOS 安装配置Sublime Text
date: 2017-10-09 20:42
---

# macOS 安装配置Sublime Text

>macOS 10.12.6
>macOS 10.13

#### 安装[Package Control](https://packagecontrol.io/)
The console is accessed via the ctrl+` shortcut or the View > Show Console menu.

#### 查看安装命令[Package Control Installtion](https://packagecontrol.io/installation)
```python
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

#### 安装插件
via cmd + shift +p

input "install package"

##### [Emmet](https://emmet.io/)
```

```

##### [Pretty JSON](http://blog.csdn.net/billfeller/article/details/39899435)
```
常用快捷键说明
ctrl+alt+j 格式化json字符串
ctrl+alt+m 压缩json字符串
```
##### [HTML-CSS-JS Prettify](https://github.com/victorporof/Sublime-HTMLPrettify)

```
#格式化html、css、js
command+shift+h 格式化快捷键
```
##### [P3-Assembly-Sublime-Text-Syntax](https://github.com/Jguer/P3-Assembly-Sublime-Text-Syntax)