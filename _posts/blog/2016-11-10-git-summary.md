---
layout: post
layout: post
description: git提交代码流程的总结
title:  git提交代码
category: blog
---

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

-----------------------我是分割线哈-------------------------
    
    git branch -vv 	-- 查看本地分之对应在远程追踪分支

    git pull --rebase	-- 1把本地repo.从上次pull之后的变更暂存起來 2恢复到上次pull时的状态 3合并远端的变更到本地 4最后再合并刚刚暂存下來的本地变更

    git rebase --continue	-- 解决冲突后
   
    git config --global alias.别名 "命令"	-- 设置命令别名

    git reset --hard 	--hard（回滚所有操作） --mixed（回滚commit和add操作） --soft（回滚commit操作） HEAD^（回滚到上一个版本）

    git reset --hard HEAD 	-- 放弃本地修改

    git stash [save "message"]	-- 存草稿

    git stash list 	-- 查看草稿列表

    git stash pop 	-- 把最顶层在草稿应用到当前分支，并且删除顶层草稿

    git stash pop --index stash@{0} 	--index 参数：不仅恢复工作区，还恢复暂存区,<stash>指定恢复某一个具体进度。如果没有这个参数，默认恢复最新进度

    git stash apply stashId 	-- 应用草稿中的改动到当前分支（不删除草稿）

    git stash apply [--index] [<stash>] 不删除已恢复的进度

    git stash drop [<stash>] 删除某一个进度，默认删除最新进度 

    git stash clear 删除所有进度 

    git log --oneline --graph	-- 以图形查看日志

    git remote -v	-- 查看资源库址
      
### 几点注意：

#####   1,在git review -d 9115 新建的分支上修改好了后 不要再次commit，否则最造成两次commit  不能review。

#####   2,需要把你的changes加到暂存区里 （git add .）再--amend，否则你的改变没有在commit中。

#####   3，git bash 选中之后出现^C并且换行的情况可以按住shift选中就能选中复制了
  