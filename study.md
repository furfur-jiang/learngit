### study git

注：本文参考自廖雪峰官网

创建一个文件名为learngit`mkdir learngit`
注：需要上传的文件需要放在该文件目录下或子目录下

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：
1. 使用命令`git add xxx文件`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m “文件说明”`，完成。

可以让我们时刻掌握仓库当前的状态`git status`
查看修改情况 `git diff xxx文件`