---
title:  "Git版本管理工具"
categories: [前端工具]
tags: [git, svn]
---
## 工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

**工作区（Working Directory）：就是电脑里能看到的目录**  
**版本库（Repository）： 工作区的一个隐藏目录.git**

Git的版本库里存了很多东西，其中最重要的就是称为`stage`（或者叫`index`）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![GIT工作区和版本库](https://static.liaoxuefeng.com/files/attachments/919020037470528/0)


## 创建仓库

`git init` 创建、初始化一个git仓库，也就是把当前目录变成Git可以管理的仓库

## 添加文件

`git add` 把文件修改添加到暂存区  
`git commit` 把文件提交到分支 `-m` 本次提交的说明  
`git status` 查看仓库状态  
`git diff` 查看修改内容  

## 版本回退

`git log` 查看提交日志 `--pretty=oneline` 单行模式  
`git reset <commit_id>`  切换到指定版本 `--hard`   
`git reflog` 查看历史命令  

## 管理修改
Git跟踪并管理的是修改，每次修改，如果不add到暂存区，那就不会加入到commit中  

`git checkout -- <file>` 丢弃工作区修改  
`git diff HEAD -- <file>` 查看工作区和版本库文件区别  
`git reset HEAD <file>` 把暂存区的修改撤销掉（unstage），重新放回工作区  
`git rm` 删除文件

## 远程仓库（remote）
_origin_ 默认远程仓库名  
_github-path_ 远程仓库地址

- ssh: `git@`_server-name:path_ 
- https: _https://github.com/server-name/project-name.git_ 速度慢，每次推送都必须输入口令，只开放http端口的公司无法使用ssh协议而只能用https

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

`git remote add <origin> <github-path>`  关联一个远程库  
`git push <origin> master` 推送master分支的所有内容到远程仓库  `-u` 第一次推送  
`git clone <github-path>`  从远程仓库克隆

## 分支
Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

每次提交，git把他们串成一条时间线，这条时间线就是一个分支  
只有一条时间线，这个分支叫主分支（`master`）  
`HEAD`指向`master`，`master`指向提交  
![分支](http://www.liaoxuefeng.com/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0 "分支")  


创建dev分支：  
Git新建了一个指针叫dev，指向`master`相同的提交，再把`HEAD`指向dev  
![创建分支](http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0 "创建分支")  

修改dev分支：  
![修改分支](http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0 "修改分支")

合并dev分支：  
![合并分支](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0 "合并分支")

`git branch` 查看分支  
`git branch <name>` 创建分支  
`git checkout <name>` 切换分支  
`git checkout -b <name>` 创建+切换分支  
`git merge <name>` 合并某分支到当前分支  `--abort` 取消合并  `--no-ff` 禁用`Fast forward`  
`git branch -d <name>` 删除分支

### 解决冲突
当Git无法“快速合并`Fast-forward`”分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
![分别有了新的提交](http://www.liaoxuefeng.com/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0 "分别有了新的提交")

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突

![解决冲突后](http://www.liaoxuefeng.com/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0 "解决冲突后")

`git log --graph` 查看分支合并图

### 保留分支历史
合并分支时，Git会用`Fast forward`模式，这种模式下，删除分支后，会丢掉分支信息。  
强制禁用`Fast forward`模式，Git就会在`merge`时生成一个新的commit，这样，从分支历史上就可以看出分支信息。  
因为创建一个新的commit，还要加上-m参数，把commit描述写进去  
`$ git merge --no-ff -m 'merge with no-ff' <name>`

`git merge <name> --no-ff` 禁用 `Fast forward`


### 分支策略
`master`分支： 稳定，发布新版本  
`dev`分支：干活，要发布就合并到master分支  
个人分支：每人都有自己的分支，时不时往dev分支合并

![分支策略](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0 "分支策略")


### bug分支

`git stash` 保存工作现场  
`git stash list` 查看保存的工作现场  
`git stash apply | pop` 恢复现场（并删除stash内容）  

### 实验分支

`git branch -D <name>` 强行删除没有被合并过的分支


### 多人协作

`git remote` 查看远程库 `-v` 详细信息  
`git push <origin> <branch-name>` 提送修改到远程库  
`git pull` 抓取远程库  
`git checkout -b <branch-name> origin/<branch-name>` 本地创建和远程分支对应的分支  
`git branch --set-upstream <branch-name> origin/<branch-name>` 建立本地分支和远程分支的关联

**工作模式：**  
	1. 可使用`git push origin <branch-name>`推送自己的修改   
	2. 如果推送失败，因为远程分支比本地分支更新，需要`git pull`试图合并  
	3. 如果合并有冲突，则解决冲突，并在本地提交  
	4. 之后，再`git push origin <branch-name>`推送  

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`建立关联

## 标签管理
标签是版本库的一个快照，实际是指向commit的指针，但是不能移动  
标签就是一个让人容易记住的有意义的名字（v1.0），它跟某个commit(6a5819e...)绑在一起

### 创建标签
`git tag` 查看所有标签   
`git tag <name> <commit-id>` 创建标签默认为HEAD，也可以指定一个commit id补打标签   
`git tag -a <tag-name> -m` 指定标签信息 `-a` 标签名 `-m` 说明文字  
`git tag -s <tag-name> -m` 可以使用私匙签名标签  
`git show <tag-name>` 查看标签信息  

### 操作标签

`git tag -d <tag-name>` 删除一个本地标签  
`git push origin <tag-name>` 推送本地标签  
`git push origin --tags` 推送全部未推送过的本地标签  
`git push origin :refs/tags/<tag-name>` 删除一个远程标签


## 自定义Git

### 命令行颜色
`git config --global color.ui true` 命令输出适当显示不同颜色

### 忽略特殊文件

 - 参考配置，组合使用: [gitignore](https://github.com/github/gitignore)
 - 忽略某些文件时，需要编写.gitignore

### 配置别名（偷懒）
`git config alias.<alias> <git-command-name>` 配置别名 --global 对当前用户起作用，否则支队当前仓库起作用

#### 配置文件的位置
 - 当前仓库：`.git/config`文件中
 - 当前用户：用户主目录下的隐藏文件`.gitconfig`中


#### 常用配置
`git config --global alias.st status`  
`git config --global alias.co checkout`  
`git config --global alias.ci commit`  
`git config --global alias.br branch`  
`git config --global alias.last 'log -1'` 最后一次提交的信息   
`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

## 搭建Git服务器
不愿意开源，不愿意交保护费

- 搭建git服务器
- 管理公匙
- 管理权限

## 参考地址

[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)