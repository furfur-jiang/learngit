## git命令一览

注：本文参考自廖雪峰官网

### git准备

`mkdir xxx文件`创建一个文件名为xxx
注：需要上传的文件需要放在该文件目录下或子目录下

`git init`初始化一个Git仓库

添加文件到Git仓库，分两步：
1. `git add xxx文件`注意，可反复多次使用，添加多个文件；
2. `git commit -m “文件说明”`完成上传。

`git status`可以让我们时刻掌握仓库当前的状态
`git diff xxx文件`查看修改情况 

`cat xxx文件` 用于查看文件内容

### git溯源

`git log` 保存一个快照，显示从最近到最远的提交日志
注：`git log --pretty=oneline`可以查看简要版本

其中HEAD表示当前版本

`git reset --hard HEAD^`
版本回退上一个版本就是HEAD^，上上一个版本就是HEAD^^，回退100写成HEAD~100。

`git reflog`查看修改历史，回退后悔可以通过commit id（版本号）回到你原本的样子，写法如下

`$ git reset --hard 1094a`
版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。



