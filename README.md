## 初始化
1. git init 
- // 初始化仓库，将本地仓库与git建立联系

2. git clone [url,远程仓库]
- // 与远程仓库建立联系

3. git remote add origin git@github.com:Wbiokr/chatApp.git
- // 添加远程库chatApp.git

## 提交暂存区
1. git add <change files>/-all /.
- // 将本地仓库代码提交到暂存区（stage）
- // 表示保存所有变化（包括新建，修改，和删除），跟踪新文件

## 提交到分支
2. git commit -m ‘说明’
- // 提交暂存区文件[untracked]到分支
3. git commit -am 'dd' => git push 
- // 用于提交跟踪过的文件[modified]到分支
- // 如果只是改变文件的内容，直接使用该条命令

## 远程仓库
1. git push --force origin ‘branch name’
- // 强行推送到远程仓库对应分支上
- 发出pull request 
    @ githup @gitlab

2. git fetch origin master
- //从远程分支获取最新版本但不会merge

2. git pull origin master
- //从远程分支获取最新版本并merge到本地

4. git push -u origin master 
- //第一次推送本地仓库到远程仓库

5. git push origin master
- //之后的推送

6. git checkout master  => git pull 
- // 切换到主分支 获取主干代码

## 分支操作
1. git checkout -b ran
// 新建一个开发分支 ran
2. git checkout ran
// 切换到ran分支
3. git merge b
- // 将b分支合并到当前分支
4. git branch
- // 查看当前处在什么分支上
5. git branch -a 
- // 查看所有分支，包括远程
6. git branch -D ran
- // 删除本地的ran分支
7. git branch -r -d origin/ran
- // 只是删除远程分支的本地的索引，
8. 真正删除是 git push origin :ran

## 查看
4. git status
- // git add,git commit这些状态使用git status命令即可查看状态
5. git log // 查看操作日志

## 与主干同步
1. git fetch origin
2. git rebase origin/master

## 合并commit 
1. git rebase -i origin/master
- // 当分支上存在很多版本的commit 需要合并成一个 提交到远程仓库

## 版本控制
- git reset --hard HEAD^ //仓库文件回退到上一commit版本
- git reset --hard 35f69c //版本回滚到hash值35f69c开头的commit版本
- git reset HEAD a.txt //把暂存区中a.txt的修改撤销掉，放回工作区

