---
layout: post
layout: post
description: git提交代码流程的总结
title:  git提交代码
category: blog
---


## git常用命令 ##


## git提交代码流程的总结 ##

    git checkout master（切换到master分支）

    git fecth   （在master分支上取得远程最新的分支信息）

    git checkout -b S-01777 -t origin/cruise-detail (创建S-01777修改分支 追踪远程分支cruise-detail)

    git branch -vv (查看追踪信息是否正确)

    git checkout allen（切换到allen分支）

    git stash save photo（如果allen分支还没有commit分支的改变，存储allen分支的修改信息 别名为photo）

    git checkout S-01777 （切换回S-01777分支）

    git stash list  （查看存储stach的信息）

    git stash apply stach@{0} (应用刚刚存储的stach@{0}到当前的分支)

    git add .

    git commit -m "[S-01777] Phoenix - Basic Details Page "

    git review -f cruise-detail

    git commit --amend（如果review有问题就用这个命令  然后在重新提交  ）

    git review -d 9115（如果build出问题 这个命令会新建一个对应版本号的分支  在这个分支上修改好了再review会再次review到9115这个版本上去）
      
### 几点注意：

#####   1,在git review -d 9115 新建的分支上修改好了后 不要再次commit，否则最造成两次commit  不能review。
#####		2,需要把你的changes加到暂存区里 （git add .）再--amend，否则你的改变没有在commit中。
#####   3，git bash 选中之后出现^C并且换行的情况可以按住shift选中就能选中复制了
  