---
title: git命令总结
date: 2018-08-25 10:50:49
tags: git
---

有些东西真是学的快，不用的话忘的也快，博主决定抽个周六再重新回顾一下 git 常用的命令以及含义。见笑见笑～～

<!--more-->

git init				把当前目录变成git可以管理的仓库

git add 文件名		把文件添加到仓库

git commit -m “说明”	`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

git status			`git status`命令可以让我们时刻掌握仓库当前的状态

git diff				如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

git log				显示从最近到最远的提交日志，如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：只显示commit id

git reset				回退版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

eg：`git rest --hard HEAD^`    `--hard`参数

git  reflog			用来记录你的每一次命令,以便回退版本后确定要回到未来的哪个版本。

git checkout -- 文件名 	把文件在工作区的修改全部撤销，这里有两种情况：

一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

git reset HEAD <file>	可以把暂存区的修改撤销掉（unstage），重新放回工作区

git rm <file> 			从版本库中删除该文件

git remote add origin git@github.com:<github账户名>/<仓库名>.git    把一个已有的本地仓库与github仓库关联

git push -u origin master	把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命命令。此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

git clone				从远程库克隆一个本地库

git chekout -b dev	创建`dev`分支  `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令`git branch dev`，`git checkout dev`

git branch			查看当前分支

git branch <name>	创建分支

git checkout <name>	切换分支

git merge dev		把`dev`分支的工作成果合并到`master`分支上，`git merge`命令用于合并指定分支到当前分支
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

git branch -d dev 		删除`dev`分支

git log --graph --pretty=oneline --abbrev-commit	查看分支的合并情况，用git log --graph命令可以看到分支合并图。
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

git stash			可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash list		      查看存储的工作现场
git stash apply		   恢复工作现场，但是恢复后，stash内容并不删除，需要用git stash drop来删除
git stash pop		 恢复的同时把stash内容也删了
git branch -D <name> 如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
git remote			查看远程库的信息，用git remote -v显示更详细的信息：
git push origin <分支名> 推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支
git branch --set-upstream-to=origin/dev dev   设置dev和origin/dev的链接
git pull	把最新的提交从origin/dev抓下来（解决冲突可用）
因此，多人协作的工作模式通常是这样：
1. 首先，可以试图用git push origin <branch-name>推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。
没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！