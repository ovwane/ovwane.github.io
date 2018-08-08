---
date: 2018-08-08 20:16:02
---

## 安装hexo
```
nvm install 8.9.1
npm install hexo-cli -g

hexo version
```


### 生产sshkey
```
ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f ~/.ssh/hexo_rsa

#github和coding.net都是设置的只有这个项目可以使用这个公钥
```
```
vim ~/.ssh/config
# github.com
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/hexo_rsa

# git.coding.net
Host git.coding.net
HostName git.coding.net
PreferredAuthentications publickey
IdentityFile ~/.ssh/hexo_rsa
```

### 测试
```
ssh -T git@github.com
ssh -T git@git.coding.net
```

### 初始化blog分支
```
cd ~/projects

hexo init blog.ovwane.me
cd blog.ovwane.me
npm install

初始化git
git clone -b hexo git@github.com:ovwane/ovwane.github.io.git blog.ovwane.me
git init
git checkout -b hexo
git submodule add https://github.com/ovwane/hexo-theme-spfk.git themes/spfk

git add .
git commit -m "初始化hexo分支"
git remote add origin git@github.com:ovwane/ovwane.github.io.git

git push origin hexo
```

### 更改主题为spfk
```
cd ~/projects/blog.ovwane.me
rm -rf themes/landscape
#sed -i 's/theme: landscape/theme: spfk/g' _config.yml

git add .
git commit -m "删除默认主题landscape，然后修改主题为spfk"
git push origin hexo
```

### 修改_config.yml 和 主题的 _config.yml替换主题的图片

### 配置travis
```
cd ~/projects/blog.ovwane.me
mkdir .travis
touch .travis.yml
brew install travis
travis login --auto
travis encrypt-file ~/.ssh/hexo_rsa --add 
mv hexo_rsa.enc .travis/
```

### .travis/ssh_config
```
vim .travis/ssh_config
# github.com
Host github.com
	HostName github.com
	User git
	StrictHostKeyChecking no
	IdentityFile ~/.ssh/hexo_rsa
	IdentitiesOnly yes
	
# git.coding.net
Host git.coding.net
	HostName git.coding.net
	User git
	StrictHostKeyChecking no
	IdentityFile ~/.ssh/hexo_rsa
	IdentitiesOnly yes
```

```
cd ~/projects/blog.ovwane.me
git add .
git commit -m "添加travis ci相关文件"
git push origin hexo
```

```
vim _config.yml
title: 幻舞梦境
author: 幻舞梦境
url: http://ovwane.me
skip_render:
  - README.md
 
deploy:
  type: git
  repo:
      github: git@github.com:ovwane/ovwane.github.io.git
      coding: git@git.coding.net:ovwane/ovwane.coding.me.git
  branch: master
```
```
cd ~/projects/blog.ovwane.me
git add .
git commit -m "修改_config.yml添加deploy信息"
git push origin hexo
```

### .travis.yml
```
language: node_js
sudo: false
node_js:
- 8.9.1
branches:
  only:
  - hexo
cache:
  apt: true
  yarn: true
  directories:
  - node_modules
before_install:
- openssl aes-256-cbc -K $encrypted_b6a9c7fbd133_key -iv $encrypted_b6a9c7fbd133_iv -in .travis/hexo_rsa.enc -out ~/.ssh/hexo_rsa -d
- chmod 600 ~/.ssh/hexo_rsa
- eval $(ssh-agent)
- ssh-add ~/.ssh/hexo_rsa
- cp .travis/ssh_config ~/.ssh/config
- git config --global user.name "Jinlong Quan"
- git config --global user.email "ovwane@gmail.com"
install:
- npm install hexo-cli -g
- npm install hexo-deployer-git --save
- npm install
- git clone https://github.com/ovwane/ovwane.github.io .deploy_git
- cd .deploy_git && git checkout master
- cd ..
script:
- hexo clean
- hexo generate
- hexo deploy
after_success:
- echo "OK!网站部署成功啦！"
```
```
cd ~/projects/blog.ovwane.me
git add .
git commit -m "添加.travis.yml打包命令"
git push origin hexo
```
```
- git clone https://github.com/ovwane/ovwane.github.io .deploy_git
- cd .deploy_git && git checkout master
- cd ..
```
```
cd ~/projects/blog.ovwane.me
git add .
git commit -m "添加.travis.yml打包命令，并添加拉取master分支，防止每次更新全部文件。"
git push origin hexo
```

### 参考


[使用 Travis CI 自動發布 Hexo 內容到 Github](https://soarlin.github.io/2017/03/29/use-travis-ci-auto-deploy-to-github/)
[使用Travis CI自动构建hexo博客](http://magicse7en.github.io/2016/03/27/travis-ci-auto-deploy-hexo-github/)
[使用Travis CI自动部署Github/Coding Pages博客
](https://imzlp.me/posts/42318/)
[#232 Hexo + GitHub + Travis CI + VPS 自动部署](https://changkun.us/archives/2017/06/232/)
[用 Travis CI 自動部屬 hexo 到 GitHub](https://ssarcandy.tw/2016/07/29/hexo-auto-deploy/)
[用 Travis CI 自动部署 hexo](http://blog.acwong.org/2016/03/20/auto-deploy-hexo-with-travis-CI/)
[使用 Travis CI 持续构建 Hexo](https://blog.nfz.moe/archives/hexo-auto-deploy-with-travis-ci.html)