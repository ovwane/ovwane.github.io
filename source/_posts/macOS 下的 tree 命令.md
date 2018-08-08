---
date: 2016-07-20 19:21:22
---

# mac下的tree命令


　　在默认安装安装的mac下没有找到tree命令，找了一下原来有个比较流氓的解决办法：
find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
　　这个命令行跑起来类似平常tree的效果，比如：
. 
|____extra 
| |____httpd-autoindex.conf 
| |____httpd-dav.conf 
| |____httpd-default.conf 
| |____httpd-info.conf 
| |____httpd-languages.conf 
| |____httpd-manual.conf 
| |____httpd-mpm.conf 
| |____httpd-multilang-errordoc.conf 
| |____httpd-ssl.conf 
| |____httpd-userdir.conf 
| |____httpd-vhosts.conf 
|____httpd.conf 
|____magic 
|____mime.types 
|____original 
| |____extra 
| | |____httpd-autoindex.conf 
| | |____httpd-dav.conf 
| | |____httpd-default.conf 
| | |____httpd-info.conf 
| | |____httpd-languages.conf 
| | |____httpd-manual.conf 
| | |____httpd-mpm.conf 
| | |____httpd-multilang-errordoc.conf 
| | |____httpd-ssl.conf 
| | |____httpd-userdir.conf 
| | |____httpd-vhosts.conf 
| |____httpd.conf 
|____other 
| |____bonjour.conf 
| |____php5.conf 
|____users 
　　写一个alias到~/.bash_profile里，就更方便了:
alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'" 
　　觉得文章有用？立即： 和朋友一起 共学习 共进步