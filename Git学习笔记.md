# Notes
个人笔记
本地仓库初始化命令

`git init`

添加远程仓库

`git remote add origin https://github.com/bolingsun/仓库名.git`

第一次把本地仓库推送到远程仓库

`git push -u origin master`

因为远程仓库是空的，所以加了-u参数

以后就可以通过`git push origin master`把本地的master分支最新修改推送的github上了。
