# Learn Git in 30 days

资源来自于 [Learn Git in 30 days](https://github.com/doggy8088/Learn-Git-in-30-days)

记录学习中的一些笔记，由于当前认知有限，可能会自以为不重要忽略掉一些内容，所以对于读者来说权当是个类似于大纲或者关键词索引之类的东西（当然忽略掉的东西以后会补起来的）

## [Day 1] 认识 Git 版本控管

> 分散式版本控制管理工具

### 学习方法

- 多使用命令行
- 找多一点的人一起学（体验多人协作）
- 分散式版本控管，每个人都有一份完整的 Repository (下面简写为 `repo`)，所以经常要合并
- 有合并就有冲突，学会解决

### Git 的几个重要设计

- 非线性开发模式（分散开式发模式）
    - 分支与合并机制
    - 历史变更路径
- 分散式开发模型
    - 每个人都有完整的开发历史记录
    - 每个人都有完整的 repo
    - 变更过的档案与历史都`仅在本地 repo`
- 兼容现有操作系统
    - Git 其实就是个文件夹加上一些配置
    - Git 发布方式：HTTP，FTP，rsync，SSH 等
- 高效处理大型文件
- 历史记录保护
    - hash id
- 以工具集为主的设计
- 弹性的合并策略
- 被动的垃圾回收机制
    - 可以中断当前操作或者恢复上一个操作
    - 手动清理无用文件：`git gc --prune`
- 定期封装物件
    - 可以将老旧的档案自动封装进一个封装档(packfile)中，以改善档案储存效率
    - 手动封装：`git gc`
    - 检查档案系统是否完整：`git fsck`

## [Day 2] 在 Windows 平台必裝的三套 Git 工具

- Git for Windows
- GitHub for Windows
- SourceTree

### Git for Windows

- 查看 Git 版本：`git --version`

### GitHub for Windows

- GitHub SSH key

**注意：**

 - 在 PowerShell 中 `{}` 有特殊的意义，所以如果 git 参数用到 `{}` 的话，要在该参数的前后加上'单引号'

### SourceTree

这个的界面真实相当不错呀

### TortoiseGit

## [Day 3] 建立储存库

- local repo
- shared repo
- remote repo

### 本机建立本地储存库(local repo)

- 预设资料夹 ` %USERPROFILE%\Documents\GitHub`
- 工作目录 `working directory`
- 建立储存库 `git init`

### 本机建立共用储存库(shared repo)

> 建立一个 Git 储存库，但是不包含工作目录，别名叫做 “裸储存库(bare repo)”

- 建立共用 repo：`git init --bare`

注意，这个资料夹`不能直接拿来做开发用途`，只能用来储存 Git 相关资讯，同时最好不要手工遍历这个资料夹下的文件

[不是很理解这句话的意思]当你想要建立一个工作目录时，必须先取得这个裸储存库的内容回来，这时必须使用 `git clone [repo_URI]` 复制一份回来才行，而透过 git clone 的过程，不但会自动建立工作目录，还会直接把这个裸储存库完整复制过来

使用 bare repo 的情况：

- 在一台多人使用的机器上进行协同开发，可开放大部分人对这个 bare repo 资料夹的只读权限，只让一个人或少许人才有写入权限
- 有些人会把 bare repo 放到 Dropbox 上跟自己的多台电脑同步

### 在远端建立远端储存库(remote repo)

> remote repo 和 bare repo 几乎是一样的

- 复制远端储存库：`git clone [repo_URL]`

## [Day 4] 常用的 Git 版本控管指令

- 新增 `git init`
- 删除 `git rm file`
- 重新命名 `git mv curr target`
- 提交变更 `git commit -m 'msg'`
- 查询历史记录 `git log`

### 新增

- 全部：`git add .`
- 部分：`git add file`

颜色状态：

    - Untracked 未追踪
    - Staged 准备好的档案

### 提交变更

- 提交变更：`git commit -m 'msg'`

必须拥有版本记录说明文字

### 查询历史

- 限定输出的历史记录数量：`git log -10`

### 删除

- 删除：`git rm 'file'`

注意：

    - 删除工作目录快取的 file 这个档案
    - 删除工作目录下的 file 这个**实体**档案

### 重新命名档案或目录

- 更名：`git mv currName targetName`

### 显示工作目录索引状态

- 详情：`git status`
- 精简：`git status -s`

### 重置目前工作目录

- 重置目前工作目录索引状态：`git reset`
- *工作目录*还原到最新版本：`git reset --hard`

### 还原某一个档案

把 master 分支中最新的 file 还原，同时也会复原工作目录的索引状态

当档案编辑到一般时，发现改坏了，要恢复到修改前的版本：

- 还原档案为未修改前的版本：`git checkout master file`

## [Day 5] 了解储存库、工作目录、物件与索引之间的关系

- 储存库
- 工作目录
- 资料结构
    - 物件
        - blob 物件
        - tree 物件
    - 索引

### 储存库

`.git` 文件夹就是一个完整的 Git 储存库，未来所有版本的变更都会自动存储在这个资料夹中

### 工作目录

`工作目录` 就是我们正在准备开发的文件夹，以后所有的事情都会在这个文件夹下进行

- 有些操作比如 `git checkout` 会更新工作目录下的档案
- 适时保持工作目录干净

### Git 资料结构

#### Git 物件

> 保存版本库中所有 `档案与版本记录`

物件是一个特别的档案，是将一个档案的内容取出，通过内容产生出一组 SHA1 值，然后依照这个 SHA1 值来命名一个档案

所有要进行控管的目录和档案都会先区分 `目录资讯` 和 `档案内容`，我们称为 `tree 物件` 与 `blob 物件`

blob 物件就是档案内容当成 blob 档案内容，然后进行 SHA1 计算吃 hash id，再把这个 id 当作 blob 文件名

tree 物件就是储存特定资料夹下包含哪些档案，以及该档案对应的 blob 物件的档案名为什么，tree 物件可以包含 blob 物件的档案名和相关资讯，同时也可以包含其他的 tree 物件

这些物件都储存在物件储存区，也就是储存库的 `object` 目录

#### 索引

> 保存当下 `要进版本库之前的目录状态`

索引档案为 .git 文件夹下的 `index` 文件，保存着 Git 储存库中特定版本的状态。其目的主要用来记录哪些文件要被提交到下一个 commit 中。

> Git 索引是介于物件储存区和工作目录之间的媒介

## [Day 6] 