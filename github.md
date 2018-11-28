
# 一、基本
* git：分布式版本控制系统   GitHub：面向开源及私有软件项目的托管平台
* git init              ：初始化一个仓库
* git add 文件           ：往仓库添加文件【加到暂存区】
* git commit -m "信息"    ：提交文件到当前分支
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

* git status             :掌握工作区的状态
* git log               ：查看历史纪录
* git reset --hard commit_id  ：回到某个版本 【head 指当前版本】
* git checkout -- 文件    ： 撤销此次对工作区的修改
* git rm 文件  ：用于删除工作区文件，然后git add,再git push 就可以删除GitHub上的文件

# 二、远程连接仓库

## 2.1 现有本地，后有远程
* 1）在本地创建一个仓库后，在GitHub创建一个同名仓库
* 2) git push origin master  ：将本地master分支的最新修改推送到github
## 2.2 先有远程，后有本地
* 1） 先远程创建一个仓库
* 2）克隆一个本地库：git clone http://github.com.zhaojing5340126/gitskills.git【地址】;

# 三、分支管理
* head 指向的是当前分支
* 创建分支：git branch dev;
* 切换分支：git checkout dev；
* 查看当前分支： git branch;
* 合并分支：
    * git checkout master
    * git merge dev  【合并指定分支到当前分支】
* 删除分支： git branch -d dev 
* 查看远程信息： git remote
* 推送dev分支到远程： git push origin dev
* 从远程库克隆分支： git checkout -b dev origin/dev
* 取回远程主机某个分支的更新，再与本地的指定分支合并： git pull
    * origin主机的next分支，与本地的master分支合并，需要写成下面这样 -$ git pull origin next:master
        