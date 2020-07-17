0、.gitignore文件

>当版本库中的某个文件不想进行版本控制时，可以通过.gitignore文件进行设置。
>
>git rm -r --cached .
>git add .
>git commit -m 'update .gitignore'

1、创建Git仓库

	>(1)新建一个文件夹，进入文件夹执行 git init初始化Git仓库，
	(2)添加文件到Git仓库，git add 文件名 ，可反复多次使用，添加多个文件
	(3) git commit -m "更改介绍"

2、时光机穿梭

	>(1)git status 查看仓库状态 git diff 查看修改内容
	>
	>(2)git log 查看历史记录或者 git log --pretty=oneline 
	>
	>(3)git reset --hard HEAD^   (^代表上一个版本，HEAD~100,代表第上100个版本)
	>
	>(4)git reset --hard （commit_id）  在git历史中穿梭
	>
	>(5)git log  查看被提交历史，git reflog 查看命令历史，以便确定回到哪个版本
	>
	>(6)只有git add之后的版本才能被git commit
	>
	>(7)场景1：直接丢弃工作区的修改：git checkout -- file   让file回到最近一次git commit或git add时的状态。
	>场景2：若是已经添加到暂存区：先撤回到工作区git reset HEAD <file>，然后再按场景1进行
	>场景3：已经提交了不合适的修改到版本库时，想要撤销需要版本回退。
	>
	>(8)git rm删除一个文件，git checkout 实际是用版本库里的版本替换工作区的版本



3、本地库与网上库的连接

> (1)本地Git链接网上git仓库
> git remote add origin https://github.com/liuhuanxg/learngit.git
>
> (2)将本地库与网上库链接
> git push -u origin master
>
> (3)将本地master分支的最新修改推送至GitHub
> git push origin master
>
> (4)将网上仓库复制到本地
> git clone https://github.com/liuhuanxg/gitskills.git
>
> (5)git remote -v    查看远程库的信息


4、分支管理

>将本地dev分支与远程dev分支关联
>git branch –set-upstream origin/远程分支名    本地新建分支名 
>
>(1)git checkout -b dev   创建并切换分支 
>相当于：git branch dev（创建分支） + git checkout dev（切换分支）
>
>(2)git branch 列出所有分支   当前分支前边会有*
>
>(3)git merge dev  合并分支到当前分支
>
>(4)git branch -d dev   删除dev分支
>
>(5)当分支冲突时：可以查看分支合并的情况
>git log --graph --pretty=oneline --abbrev-commit
>
>首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
>
>干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
>
>每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
>
>合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
>git merge --no-ff -m "merge with no-ff" dev
>
>(6)分支未完成不能提交但是需要去完成另一个分支时：
>先git stash然后去创建别的分支，再git stash pop
>
>(7)开发一个新功能，最好新创建一个分支，如果丢弃一个没有被合并的分支：通过git branch -D <name>强行删除

5、多人协作

	>(1)查看远程库信息，使用git remote -v；
	>
	>(2)本地新建的分支如果不推送到远程，对其他人就是不可见的；
	>
	>(3)从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
	>
	>(4)在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
	>
	>(5)建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
	>
	>(6)从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

6、标签管理

  (1)切换到需要打标签的分支上：git tag v1.0
  (2)查看所有标签：git tag
  (3)git tag v0.9 (commit id)
  (4)git show <tagname>  查看所有标签信息
  (5)git tag -a v0.1 -m "version 0.1 released" commit id   创建带有说明的标签
  (6)删除标签：git tag -d <tag name>
  (7)推送某个标签到远程：git push origin <tagname>
  (8)删除某个远程标签:git push origin :refs/tags/<tagname>