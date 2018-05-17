## 初始化
1. git init 
// 初始化仓库，将本地仓库与git建立联系
2. git clone [url,远程仓库]
// 与远程仓库建立联系
3. git checkout master  => git pull 
// 切换到主分支 获取主干代码
4. git checkout -b ran
// 新建一个开发分支 ran

## 提交分支
1. git add <change files>/-all /.
// 将本地仓库代码提交到暂存区
// 表示保存所有变化（包括新建，修改，和删除），跟踪新文件
2. git status
// 查看发生变动的文件
3. git commit -m ‘说明’
// 提交暂存区文件[untracked]
4. git commit -am 'dd' => git push 
// 用于提交跟踪过的文件[modified]
// 如果只是改变文件的内容，直接使用该条命令
5. git status
// git add,git commit这些状态使用git status命令即可查看状态
6. git log // 查看日志

## 与主干同步
1. git fetch origin
2. git rebase origin/master

## 合并commit 
1. git rebase -i origin/master
```
pick：正常选中
reword：选中，并且修改提交信息；
edit：选中，rebase时会暂停，允许你修改这个commit（参考这里）
squash：选中，会将当前commit与上一个commit合并
fixup：与squash相同，但不会保存当前commit的提交信息
exec：执行其他shell命令
```

## 推送到远程仓库
1. git push --force origin ‘branch name’
// 强行推送到远程仓库
## 发出pull request 
    @ githup @gitlab

2. git fetch origin master
//从远程分支获取最新版本但不会merge

2. git pull origin master
//从远程分支获取最新版本并merge到本地

3. git remote add origin git@github.com:Wbiokr/chatApp.git
//添加远程库chatApp.git

4. git push - u origin master 
//第一次推送本地仓库到远程仓库

5. git push origin master
//之后的推送


## 分支操作
1. git checkout ‘当前分支’
2. git merge b
// 将b分支合并到当前分支


## 版本控制
git reset --hard HEAD^ //仓库文件回退到上一commit版本

git reset --hard 35f69c //版本回滚到hash值35f69c开头的commit版本

git reset HEAD a.txt //把暂存区中a.txt的修改撤销掉，放回工作区

