
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
* 2) 然后在本地的库下面运行：git remote add origin https://github.com/zhaojing5340126/zjGoWork
* 3) git push -u origin master(如果是首次需要加-u)
* 4）如果出现
```
To github.com:zhaojing5340126/zjGoWork
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:zhaojing5340126/zjGoWork'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
   * 但输入 git pull 又出现
```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
```
是因为本地分支和远程分支没有建立联系  (使用git branch -vv  可以查看本地分支和远程分支的关联关系)  .根据命令行提示只需要执行以下命令即可:
   * git branch --set-upstream-to=origin/远程分支的名字 本地分支的名字 [建立本地分支和远程分支之间的联系]
   * 再次git pull 但又出现新问题
   ```
   fatal: refusing to merge unrelated histories
   ```
   这个问题是因为 两个 根本不相干的 git 库， 一个是本地库， 一个是远端库， 然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并,于是可以用以下命令把两段不相干的分支进行强行合并
   * git pull origin master --allow-unrelated-histories
   
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
# 四、实习时候的常用命令
```
 2、count（*）、count（1）检索表中所有记录行的数目，不论其是否包含null值
count（字段）：检索表中的这个字段的非空行数（null不算在内）
3、Git步骤：【赵静的分支：dev_car_task_list】
  3.1 git pull 【拉取代码】【git clone 。。。。】
  3.2 git stash 存储
  3.3 git pull 拉取最新的代码
  3.4 git stash pop 【它会合并代码，有冲突的话你要解决冲突】
  3.5 git push origin 。。。 上传
另： git checkout dev_car_task_list
git branch 查看分支
git branch -D dev_vehicle_update_amount_zj 删除本地未合并的分支

重点：怎么在本地创建一个新分支推送到远程
git checkout -b dev_vehicle_update_amount_zj 创建本地分支
git push -u origin dev_vehicle_update_amount_zj 将本地分支推送到远程分支【因为用了-u，所以它自己会在远程创建一个一样的分支】
之后就可以用： git push origin dev_vehicle_update_amount_zj 推送到远程

git remote set-url origin https://git.xiaojukeji.com/orion/car-share-java.git 修改origin对应的远程库【跟着的是新的远程库的地址】
git checkout -b dev origin/dev  拉取远程分支【并在本地新建分支】

https://www.cnblogs.com/ToDoToTry/p/4095626.html
https://www.cnblogs.com/ToDoToTry/p/4095626.html
https://www.cnblogs.com/chenlogin/p/6592228.html


代码给OA的流程
1、合并到merge分支【要在gitlab页面合并】
    1.1 gitlab页面：create merge request

    
    1.2 点击 change branches

    1.3 将你的 “中间分支” 合并到merge里

        1.3.1 注意事项：

            
              git checkout -b dev_vehicle_update_amount_zj2 建立中间分支
        git checkout merge    会自动从远程给你拉取并创建分支
        git pull  拉取最新的远程merge分支
         git checkout dev_vehicle_update_amount_zj2   转换到中间分支
          git merge merge  合并merge到中间分支
         git add .   可以一次add所有的
         git commit -m "result conflict”  
         git push -u origin dev_vehicle_update_amount_zj2  推送到远程中间分支
        然后你就可以在页面合并你的中间分支到merge了

2、选择你修改的目录，点进去后点击 “立即构建” 会将merge分支 发布到QA环境Jenkins 【只发布你选择的那部分】【所以如果修改了interface就要命令行进入interface目录，mvn deploy 【打包并推送到公司仓库】】
然后才立即构建】 
```
        
