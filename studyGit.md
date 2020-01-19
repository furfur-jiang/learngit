## git命令

注：本文参考自廖雪峰官网



git优点

- Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。
- Git跟踪并管理的是修改，而非文件。
- Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！

### git准备

`mkdir <file>`创建一个文件名为	`<file>`
注：需要上传的文件需要放在该文件目录下或子目录下

`git init`初始化一个Git仓库

添加文件到Git仓库，分两步：
1. `git add <file>`注意，可反复多次使用，添加多个文件；
2. `git commit -m “文件说明”`完成上传。

`git status`可以让我们时刻掌握仓库当前的状态
`git diff <file>`查看修改情况 

`cat <file>` 用于查看文件内容



### git溯源

`git log` 保存一个快照，显示从最近到最远的提交日志
注：`git log --pretty=oneline`可以查看简要版本

其中HEAD表示当前版本

`git reset --hard HEAD^`
版本回退上一个版本就是HEAD^，上上一个版本就是HEAD^^，回退100写成HEAD~100。

`git reflog`查看修改历史，回退后悔可以通过commit id（版本号）回到你原本的样子，写法如下

`$ git reset --hard 1094a`
版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

#### 工作区和暂存区

**工作区（Working Directory）**：就是在电脑里能看到的目录

**版本库（Repository）**：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

`git add <file>`把文件添加进去，实际上就是把文件修改添加到**暂存区**；

`git commit -m"xxx"`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

<u>你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。</u>

注：Untracked files 未添加到暂存区

#### 管理修改

<u>为什么Git比其他版本控制系统设计得优秀，因为**Git跟踪并管理的是修改**，而非文件。</u>

#### 撤销修改checkout

未保存到暂存区：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- <file>`。

已保存到暂存区：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file> `，第二步`git checkout -- <file>`  

注意：`git checkout -- <file>` 两个杠后面有个空格

#### 删除文件rm

`git rm <file>`+`git commit <file>`

如果一个文件已经被提交到版本库，那么你永远不用担心误删，可以通过`git checkout -- <file>` 恢复，但是你只能恢复文件到**最新版本**你会**丢失最近一次提交后你修改的内容**。

### 远程仓库

创建SSH Key，用户主页面有则不需要创建

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

![image-20200118194309805](C:\Users\JMQ\Pictures\素材\查看.png)

为什么GitHub需要SSH Key呢？

因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

#### 关联remote

关联远程仓库使用原生git协议较快

```
$ git remote add origin git@github.com:xxx(你的github账户名)/xxx(仓库名).git
```
或者，但是用https会比较慢

```
$ git remote add origin https://github.com/furfur-jiang/studyGit.git
```

#### 推送push

把本地库的内容推送到远程（第一次push需要加上-u）

```
$ git push -u origin master  
```

实际上是把当前分支`master`推送到远程。加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

#### 克隆clone

```
$ git clone git@github.com:xxx(你的github账户名)/xxx(仓库名).git
```

### 管理分支

Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

#### 分支代码

查看分支：`git branch`   当前分支前面会标一个`*`号

创建分支：`git branch <name> `

切换分支：`git checkout <name>   `或者`git switch <name> `

创建+切换分支：`git checkout -b <name> `或者`git switch -c <name> `

合并某分支到当前分支：`git merge <name> `

删除分支：`git branch -d <name> `

#### 分支实战

[分支实战详解]: https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424

1. 创建`dev`分支，然后切换到`dev`分支：`git checkout -b dev`

2. 正常提交：add,commit
3. 回到master分支：`git checkout master`

4. `dev`分支的工作成果合并到`master`分支：`git merge dev`
5. 删除`dev`分支`git branch -d dev`

#### 分支冲突

分支的合并情况查看 :

```
git log --graph --pretty=oneline --abbrev-commit
```

#### 分支策略

如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

禁用`Fast forward` `--no-ff`方式的`git merge`：

1. 创建并切换`dev`分支：`git switch -c dev`

2. 修改文件，并提交
3. 切换回`master`：`git switch master`

4. 合并`dev`分支，请注意`--no-ff`，表示禁用`Fast forward`：`git merge --no-ff -m "merge with no-ff" dev`

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

#### bug分支

注意：要跳转到别的分支，必须先把原来的工作commit或者stash储存起来

`git stash`可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

`git stash list`查看保存的现场

恢复现场方法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

二是用`git stash pop`，恢复的同时把stash内容也删了

可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：`git stash apply stash@{0}`

Git专门提供了一个`git cherry-pick <commit>`命令，让我们能复制一个特定的提交到当前分支

#### Feature分支

增加新功能的实验性代码，最好新建一个分支

要丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除。

#### 多人协作

`git remote`要查看远程库的信息

`git remote -v`显示更详细的信息

本地新建的分支如果不推送到远程，对其他人就是不可见的；你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，创建本地`dev`分支：`git branch --set-upstream branch-name origin/branch-name`

团队开发流程：

1. 首先，可以试图用`git push origin <branch-name> `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。
4. 如果合并有冲突，则解决冲突，并在本地提交commit；
5. 用`git push origin <branch-name>`推送就能成功！

#### Rebase

`git rebase`可以把本地未push的分叉提交历史整理成直线；缺点是本地的分叉提交已经被修改过了，最后通过push操作把本地分支推送到远程。

### 标签管理

`git tag <name> `打一个新标签，默认为HEAD，也可以指定一个commit id
`git tag`查看所有标签
`git show <name>`查看标签信息，标签不是按时间顺序列出，而是按字母排序的

创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字，指定commit id的`git tag <name> <commit id>`    eg：

```
git tag -a v0.1 -m "version 0.1 released" 1094adb
```

`git push origin <name> `推送某个标签到远程

`git push origin --tags`一次性推送全部尚未推送到远程的本地标签

`git tag -d <name>`删除本地标签,创建的标签都只存储在本地，不会自动推送到远程

标签已经推送到远程，先从本地删除`git tag -d <name>`，再从远程删除`git push origin :refs/tags/<name>`

### 使用GitHub

如何参与别人的项目eg bootstrap

点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone

注意：一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。

### 使用码云

国内的Git托管服务，码云也提供免费的Git仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。

使用命令`git remote add`把它和码云的远程库关联：

```
git remote add origin git@gitee.com:liaoxuefeng/learngit.git
```

在使用命令`git remote add`时报错，本地库已经关联了一个名叫`origin`的远程库，此时，可以先用`git remote -v`查看远程库信息，可以删除已有的GitHub远程库`git remote rm origin`

如何既关联GitHub，又关联码云？

使用多个远程库时，我们要注意，git给远程库起的默认名称是`origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

### 自定义git

让Git显示颜色，会让命令输出看起来更醒目：

```
$ git config --global color.ui true
```

#### 忽略特殊文件

特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

`.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

`.gitignore`配置参考https://github.com/github/gitignore

注意：在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

你确实想添加一个被忽略的文件，可以用`-f`强制添加到Git

或者用`git check-ignore`命令检查`.gitignore`写的问题，Git会告诉我们，`.gitignore`的第几行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

#### 配置别名

以后`st`就表示`status`

```
git config --global alias.st status
```

命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个`unstage`别名：

```
$ git config --global alias.unstage 'reset HEAD'
```

把`lg`配置成了各种color：

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

#### 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。每个仓库的Git配置文件都放在`.git/config`文件

#### 搭建Git服务器

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。你已经有`sudo`权限的用户账号

第一步，安装`git`：

```
$ sudo apt-get install git
```

第二步，创建一个`git`用户，用来运行`git`服务：

```
$ sudo adduser git
```

第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

```
$ sudo git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

```
$ sudo chown -R git:git sample.git
```

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：

```
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```

剩下的推送就简单了。

##### 管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的`/home/git/.ssh/authorized_keys`文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

##### 管理权限

有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。[Gitolite](https://github.com/sitaramc/gitolite)就是这个工具。

### 使用SourceTree

Git有很多图形界面工具，这里我们推荐[SourceTree](https://www.sourcetreeapp.com/)，它是由[Atlassian](https://www.atlassian.com/)开发的免费Git图形界面工具，可以操作任何Git库。

使用说明：https://www.liaoxuefeng.com/wiki/896043488029600/1317161920364578