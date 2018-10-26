# 一定要搞定GIT

1.直接记录快照，而非差异比较
2.近乎所有操作都是本地执行，本地git数据库
3.git保证完整性(Git 数据库中保存的信息都是以文件内容的哈希值来索引)
4.git一般只添加数据，git绝大部分的操作都是可逆的

## 三种状态

1. 已提交：committed  => 已安全提交到本地数据库
2. 已修改：modified  => 修改了文件，未保存到本地数据库中
3. 已暂存：staged  => 对已修改的文件做了版本标记，使之包含在下次提交快照里 

## 三个区域

- 工作目录 =>              暂存区域 =>          git仓库目录
- Working Directory  ==> Staging Area  ==> .git directory (Respository)
- (修改文件)  git add/commit (文件快照放入暂存区) git push => 入仓

- 工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
- 暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作‘索引’，不过一般说法还是叫暂存区域。
- Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方，从其它计算机克隆仓库时，拷贝的就是这里的数据。

## git内部原理

### .git 

1.当在一个新目录或已有目录执行 git init 时，Git 会创建一个 .git 目录。
2..git目录包含了几乎所有存储和操作的对象,(若想拷贝一个版本库,只需拷贝这个目录)
3..git目录结构:
  - HEAD        => 指示目前被检出的分支
  - objects/    => 存储所有数据内容
  - refs/       => 储指向数据（分支）的提交对象的指针
  - index       => 文件保存暂存区信息
  - config*    => config 文件包含项目特有的配置选项
  - description => 仅供 GitWeb 程序使用
  - hooks/    => 包含客户端或服务端的钩子脚本（hook scripts）
  - info/    => 包含一个全局性排除（global exclude）文件，用以放置那些不希望被记录在 .gitignore 文件中的忽略模式(ignored patterns)

### git对象- objects

1. (存)git是一个内容寻址文件系统, 意味着，Git 的核心部分是一个简单的键值对数据库（key-value data store）
  - before: 
    - info
    - pack
  - $ echo 'test content' | git hash-object -w --stdin 
  -   > bd83801f897f05a09e1c286e20d4c50da0890952
  - (-w 指示hash-object命令存储数据对象, --stdin 选项则指示该命令从标准输入读取内容) = 该命令输出一个长度为 40 个字符的校验和)

  - after: .git/objects
    - info
    - pack
    - bd/83801f897f05a09e1c286e20d4c50da0890952

2. (取)$git cat-file -p bd83801f897f05a09e1c286e20d4c50da0890952
  - > test content
  - cat-file 命令从git那里取回数据, -p选项只是该命令自动判断内容的类型,并显示格式友好的内容

3. 简单版本控制

  - echo 'version1' > test.txt => 创建一个文件 并向文件里写入version1
  - git hash-object -w test.txt => 将内容存入数据库
  - echo 'versio2' > test.txt => 修改test.txt文件,将内容改成version2
  - git hash-object -w test.txt => 再次存入数据库
  - find .git/objects -type f 
    - .git/objects/1f/7a7a472abf3dd9643fd615f6da379c4acb3e3a  => version2
    - .git/objects/83/baae61804e65cc73a7201a7252750c76066a30 => version1
    - .git/objects/bd/83801f897f05a09e1c286e20d4c50da0890952 
  - git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30 > test.txt => 将版本恢复至 version1
  - cat test.txt => 显示test.txt内容 > version1
  - git cat-file -t 83baae61804e65cc73a7201a7252750c76066a30 =>显示内部存储的对象类型,只要给定对象的SHA-1的值
  - > blob (数据对象)  
  - ------------------以上操作只是保存的文件的内容,文件名并没有被保存----------------------------

4. 树对象 (tree object)
  - 解决文件名保存问题 以及 允许我们多个文件组织到一起
  - git内容均以树对象和数据对象的形式存储，其中树对象对应了UNIX 中的目录项，数据对象则大致上对应了 inodes 或文件内容。
  - 一个树对象包含了一条或多条树对象记录（tree entry），每条记录含有一个指向数据对象或者子树对象的 SHA-1 指针，以及相应的模式、类型、文件名信息。
  -   创建暂存区   把test文件人为加入一个新的暂存区  将文件添加到git数据库中 文件模式 SHA-1 和 文件名
  - $ git update-index --add --cacheinfo 100644\83baae61804e65cc73a7201a7252750c76066a30 test.txt
  - $ git cat-file -p master^{tree}
  - > 100644 blob 83baae61804e65cc73a7201a7252750c76066a30    test.txt
  - master^{tree} 语法表示 master 分支上最新的提交所指向的树对象
  - 合法模式: 100644 普通文件类型 100755 一个可执行文件  120000 一个符号链接

5.创建树
  (1). $ git write-tree 命令将暂存区内容写入一个树对象
      - > d8329fc1cc938780ffdd9f94e0d364e0ea74f579
  (2). $ echo 'new file' > new.txt

6. 提交对象
7. 对象存储

## GIT 引用 

1. HEAD 引用
  - HEAD 文件是一个符号引用（symbolic reference），指向目前所在的分支。
  - 指针指向当前分支的引用路径 - refs/heads/master

2. 标签引用 /refs
  - 标签对象 /refs/tag
    - 类似于一个提交对象——它包含一个标签创建者信息、一个日期、一段注释信息，以及一个指针。
    - 标签对象通常指向一个提交对象，而不是一个树对象
    - 轻量标签 - 一个固定的引用
    - 附注标签 - Git 会创建一个标签对象，并记录一个引用来指向该标签对象，而不是直接指向提交对象。

3. 远程引用 /refs/remote （remote reference）
  - 你添加了一个远程版本库并对其执行过推送操作
  - git push 后远程版本库的master分支所对应的SHA-1值,就是最近一次与服务器通信的时本地分支所对应的SHA-1值
  - 远程引用和分支（位于 refs/heads 目录下的引用）之间最主要的区别在于，远程引用是只读的
  - 可以 git checkout 到某个远程引用，但是 Git 并不会将 HEAD 引用指向该远程引用。
  - 你永远不能通过 commit 命令来更新远程引用。
  -  Git 将这些远程引用作为记录远程服务器上各分支最后已知位置状态的书签来管理。

## 包文件 
- $ git gc 
  - .git/objects/info/packs
  - .git/objects/pack/pack-c71fc510f75681633288d462c6c70f1cd981c241.idx
  - .git/objects/pack/pack-c71fc510f75681633288d462c6c70f1cd981c241.pack

  
## 引用规格 .git/config 
  - $ git remote add origin https://github.com/schacon/simplegit-progit
  - 上述命令会在你的 .git/config 文件中添加一个小节，并在其中指定远程版本库的名称（origin）、URL 和一个用于获取操作的引用规格（refspec）：
  ```
    [remote "origin"]
    url = https://github.com/schacon/simplegit-progit
    fetch = +refs/heads/*:refs/remotes/origin/*     // git fetch 的引用规格
  ```
  - 用规格的格式由一个可选的 + 号和紧随其后的 <src>:<dst> 组成，其中 <src> 是一个模式（pattern），代表远程版本库中的引用；<dst> 是那些远程引用在本地所对应的位置。
  - 引用规格由 git remote add 命令自动生成， Git 获取服务器中 refs/heads/ 下面的所有引用，并将它写入到本地的 refs/remotes/origin/ 中。

  1. 引用规格推送
    - 如何将QA团队的master分支推送到远程服务器qa/master分支上
    - > git push origin master:refs/heads/qa/master

## 删除引用 - 远程
  1.git push origin  : qa/master
  2.git push origin --delete qa-master