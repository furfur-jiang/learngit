## git命令一览

注：本文参考自廖雪峰官网

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

### 工作区和暂存区

**工作区（Working Directory）**：就是在电脑里能看到的目录

**版本库（Repository）**：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

`git add <file>`把文件添加进去，实际上就是把文件修改添加到**暂存区**；

`git commit -m"xxx"`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

<u>你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。</u>

注：Untracked files 未添加到暂存区

### 管理修改

<u>为什么Git比其他版本控制系统设计得优秀，因为**Git跟踪并管理的是修改**，而非文件。</u>

### 撤销修改

未执行add：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- <file>`。

执行add：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file> `，第二步`git checkout -- <file>`  

注意：`checkout -- <file>` 两个杠后面有个空格

