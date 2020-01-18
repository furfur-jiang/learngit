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

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号

`git merge`命令用于合并指定分支到当前分支

Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

#### 分支实战

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'

加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev
$ git checkout dev
```

然后，用`git branch`命令查看当前分支：

```
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交

```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```
$ git checkout master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```
$ git branch
* master
```