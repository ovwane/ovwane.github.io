date: 2013-05-11 12:48



### git常用命令

```shell
#克隆仓库
git clone git@github.com:ovwane/ovwane.github.io.git
#创建分支
git checkout -b blog
#切换分支
git checout blog
#查看分支
git branch
#查看所有分支
git branch -a
#删除分支
git push origin blog
git push origin -d blog
#删除子模块
git rm -r --cached themes/spfk
```

### 撤销操作

```shell
# 查看之前提交的git commit的id 
$ git log 
# 完成撤销,同时将代码恢复到前一commit_id 对应的版本 
$ git reset --hard id
# 完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改
git reset id
```

