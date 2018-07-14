date: 2018-05-08 17:01

> CentOS Linux release 7.5.1804 (Core)
>
> Linux 3.10.0-862.3.3.el7.x86_64

**安装依赖**

```shell
yum install -y mercurial gcc bison
```

**安装**[gvm](https://github.com/moovweb/gvm)

```shell
curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer | zsh
```

Go 实现了自举（用 Go 编译 Go），就需要用到 Go 1.4 来做编译

```shell
# -B 表示只安装二进制包
$ gvm install go1.4.3 -B
$ gvm use go1.4.3
$ export GOROOT_BOOTSTRAP=$GOROOT
$ gvm install go1.10.2
```

**离线安装**

```shell
# git clone 官方源go1.10.2
git clone -b go1.10.2 https://go.googlesource.com/go go1.10.2
#可以放到别的电脑上
tar cvfz go1.10.2.tar.bz2 go1.10.2
#指定本地go1.10.2 git源
gvm install go1.10.2 --source=/root/go1.10.2
```

[[解决 golang在macos编译时fatal error: MSpanList_Insert错误]](https://github.com/moovweb/gvm/issues/264)|[gvm install go1.9.2 fails on macOS 10.12.6](https://github.com/moovweb/gvm/issues/284)

安装好之后，指定默认使用这个版本，加上 `--default` 即可，省去每次敲 `gvm use`

```shell
$ gvm use go1.10.2 --default
```

**使用**

```shell
# 查看可以安装的go版本
$ gvm listall
# 查看本地安装的go版本
$ gvm list
# go环境变量
$ go env
```

### 参考

[使用gvm管理多版本golang](http://chen-tao.github.io/2017/09/14/Use-gvm-manage-golang-version/)

[go依赖包管理工具对比](https://ieevee.com/tech/2017/07/10/go-import.html)

[Go 语言多版本安装及管理利器 - GVM](https://gocn.io/article/107)