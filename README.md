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

