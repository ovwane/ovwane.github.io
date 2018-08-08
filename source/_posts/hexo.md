---
date: 2018-08-08 16:41:01
---

ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f ~/.ssh/github_rsa 

ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f ~/.ssh/coding_net_rsa 



```shell
vim ~/.ssh/config

Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github_rsa
  
Host git.coding.net
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/coding_net_rsa

```





```shell
$ git clone -b hexo git@github.com:ovwane/ovwane.github.io.git blog.ovwane.me
```