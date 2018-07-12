老男孩教育Python课程老师们的博客

Alex

武沛齐
[Mr.Seven](http://www.cnblogs.com/wupeiqi/)


[老铁，这年头不会点Git真不行！！！](http://www.cnblogs.com/wupeiqi/p/7295372.html)

[Tech Talk: Linus Torvalds on git](https://www.youtube.com/watch?v=4XpnKHJAok8) 4:36 git发音 给特 

[How do you pronounce “Git”?](https://english.stackexchange.com/questions/47622/how-do-you-pronounce-git?newreg=e31e4c18b6564225aecf3b0cfe260ebf)



[Python 面向对象（初级篇）](http://www.cnblogs.com/wupeiqi/p/4493506.html)
[python 面向对象（进阶篇）](http://www.cnblogs.com/wupeiqi/p/4766801.html)

```
#配置全局用户和邮箱
git config --global user.name ""
git config --global user.email ""
#初始化
git init
#查看git状态
git status
#添加当前目录所有文件到版本库
git add .
#提交到版本库，并填写版本说明。
git commit -m "提交说明"
#查看提交记录
git log
#现在b版本，恢复到a版本本，“4ba728de5e2136e03124732fc3e6c779ee74a222”这串字符是git log查看的commit后面的字符串。
git reset --hard 4ba728de5e2136e03124732fc3e6c779ee74a222

#恢复到b版本，”4ba728d“这个是查看reflog
git reflog
git reset --hard 4ba728d
```

stash

```
#将当前工作区所有修改过的内容存储到“某个地方”，将工作区还原到当前版本未修改过的状态
git stash
#查看“某个地方”存储的所有记录
git stash list
#清空“某个地方”     
git stash clear
#将第一个记录从“某个地方”重新拿到工作区（可能有冲突）
git stash pop 
#编号, 将指定编号记录从“某个地方”重新拿到工作区（可能有冲突）
git stash apply
#编号，删除指定编号的记录 
git stash drop
```

branch

```
#创建分支
git branch dev
#切换到dev分支
git checkout dev
#创建并切换到指定分支
git branch -m dev
#查看所有文章
git branch
#删除分支
git branch -d dev
#将指定分支合并到当前分支
git merge dev
```

远程

git pull = git fetch + git merge

```
#添加远程git
git remote add origin
#把代码推送到远程git
git push origin dev
#拉取代码,并合并到本地
git pull origin dev
#获取分支最新内容到版本库的分支
git fetch origin dev
#将版本库的分支内容合并到工作去
git merge origin/dev
#初始化git代码到本地
git clone 
#创建分支且和远程分支同步
git branch dev origin/dev
```

协同合作

