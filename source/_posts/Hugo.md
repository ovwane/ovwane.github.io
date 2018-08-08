---
date: 2018-07-04 16:40:08
---

[Hugo](https://github.com/gohugoio/hugo)

[Hugo静态网站生成器中文教程](http://nanshu.wang/post/2015-01-31/)

```bash
go get github.com/magefile/mage
go get -d github.com/gohugoio/hugo
cd ${GOPATH:-$HOME/go}/src/github.com/gohugoio/hugo
mage vendor
mage install
```

新建hugo项目

`hugo new site hugo.ovwane.me`

初始化git

```shell
cd hugo.ovwane.me
git init
```

Install DocDock as git submodule

`git submodule add https://github.com/vjeantet/hugo-theme-docdock.git themes/hugo-theme-docdock`



`git submodule add https://github.com/spf13/hugoThemes.git themes/spf13`



Next initialize submodule for parent git repo:

```shell
git submodule init
git submodule update
```

Configure
Import sample config from sample site to Hugo root.

`cp themes/hugo-theme-docdock/exampleSite/config.toml .`

Preview site

`hugo server`



`hugo new post/first-article.md`

`git clone https://github.com/vjeantet/hugo-theme-docdock.git themes/docdock`





git checkout -b hugo

git add .

git commit -m "初始化分支，主题使用psf13的"

git push --set-upstream origin hugo