# 1 说说你对版本管理的理解？常用的版本管理工具有哪些？
## 1.1 是什么
- 版本控制是一种软件工程技巧，借此能在软件开发的过程中，确保由不同人所编写的同一程序文件都得到同步。
- 通过文档控制，能记录任何工程项目内各个模块的改动历程，并为每次改动编上序号。
- 版本控制能提供项目的设计者，将设计恢复到之前任一状态的选择权。
## 1.2 有哪些
1. 本地版本控制系统
- 优：适合管理文本，如系统配置
- 缺：不支持远程操作，不适合多人版本开发
2. 集中式版本控制系统：SVN、CVS

存在一个单一的中央服务器，所有的版本历史和文件数据都存储在这个中央服务器上。开发人员需要从中央服务器拉取数据，并在修改之后提交回中央服务器。
- 优：适合多人团队协作开发；代码集中化管理
- 缺：单点故障；必须联网，无法单机工作
3. 分布式版本控制系统：Git、HG

每个开发人员的工作空间都会有一个完整的代码库的拷贝，包括完整的历史版本记录。这些本地仓库之间可以通过网络进行交互，但每一个本地仓库都是一个独立的版本控制系统。
- 优：适合多人团队协作开发；代码集中化管理；**可以离线工作**；**每个计算机都是一个完整仓库**

使用 Git 时，常见的工作流程涉及以下几个步骤：
- 工作区：开发者在这里进行实际的代码修改。它是**当前目录下可见的文件和文件夹**。
- 暂存区：修改完成后，可以通过 git add <file> 将修改从工作区添加到暂存区。暂存区**是一个集合区域，保存了将要提交到本地仓库的快照**。
- 本地仓库：使用 git commit -m "message" 命令**提交暂存区的快照到本地仓库，记录一系列历史版本**。
- 远程仓库：使用 git push <remote> <branch> 命令，**将本地仓库的提交上传到远程仓库**，便于其他开发者进行协作。
- 同步远程变更：使用 git pull 或 git fetch 命令，**从远程仓库获取最新的提交并合并到本地**。

```
# 克隆远程仓库到本地
git clone <repository-url>

# 创建并切换到一个新的分支
git checkout -b new-feature

# 修改代码并查看状态
git status

# 将修改添加到暂存区
git add .

# 提交进本地仓库
git commit -m "Add new feature"

# 从远程仓库获取并合并最新的变更
git pull origin main

# 将本地分支推送到远程仓库
git push origin new-feature
```

git pull
- **git pull 是一个组合命令，它实际上是 git fetch 和 git merge 的组合**。执行 git pull 后，会先从远程仓库拉取最新的提交，然后将这些提交合并到当前所在的分支。
- 如果不指定分支名称，**git pull 会默认拉取当前分支所在的远程分支**。

git status时，文件可能处于以下几种状态之一：
- 新文件（Untracked Files）：这些文件是你在工作区中新创建的，但还没有被 Git 追踪管理。这种状态指示这些文件还没有被添加到暂存区。
- 已暂存修改（Changes to be committed）：这些文件的修改已经被添加到暂存区，准备下一步提交。这些文件将被包含在下一次 git commit 中。
- 未暂存修改（Changes not staged for commit）：这些文件是工作区中的文件经过修改，但尚未添加到暂存区。需要使用 git add 命令将它们添加到暂存区。
- 删除文件（Deleted files）：这些文件在工作区中已经被删除，但删除操作还没有被添加到暂存区。需要使用 git rm 或 git add 命令将删除操作添加到暂存区。
- 已暂存的删除（Removed files to be committed）：这些文件的删除操作已经被添加到暂存区，将被包含在下一次提交中。
- 合并冲突（Unmerged paths）：在进行合并操作时，可能会遇到文件冲突，这时文件会显示为冲突状态，需要手动解决冲突。

# 2 说说你对 Git 的理解？
## 2.1 是什么
- git，是一个分布式版本控制软件，最初目的是为更好地管理Linux内核开发而设计。
- 分布式版本控制系统的客户端不止提取最新版本的文件快照，而是把代码仓库完整地镜像下来。
## 2.2 实现原理
![image](https://github.com/user-attachments/assets/6b6b212c-3da4-478a-8b76-c2dda4aed133)

1. 通过git init创建或git clone一个项目时，**项目目录会隐藏一个.git子目录，作用是用来跟踪管理版本库的**。
2. **Git中所有数据在存储前都计算校验和，然后以校验和来引用**，所以在修改和删除文件时，git能知道。
   - Git用以计算校验和的机制叫SHA-1散列（hash），这是一个由40个十六进制字符组成字符串，基于Git中文件的内容或目录结构计算出来。
3. 当修改文件时，git会修改文件的状态（已修改、已暂存、已提交）
4. 不同状态的文件在Git中处于不同的工作区域（工作区、暂存区、本地仓库、远程仓库）

## 2.3 `.git` 目录结构
以下是 `.git` 目录中常见的子目录和文件的详细介绍：
1. HEAD
   - 一个文件，指向当前所在的分支的最新提交。例如，内容可能是 `ref: refs/heads/main`，这表示当前分支是 `main`。
2. config
   - 仓库的配置文件，包含了本地和远程仓库的设置，包括远程仓库地址等。
3. description
   - 仅供 Gitweb 之类的工具使用的描述文件。对于纯 Git 操作，这个文件基本无用。
4. hooks
   - 这个目录包含客户端或服务端的钩子脚本。钩子脚本在特定事件触发时运行，比如 `pre-commit`、`post-commit` 等。
5. index
   - 索引文件，也称为暂存区，它保存了所有暂存的文件记录。
6. info
   - 包含两个子文件：`exclude` 文件，可以在这里定义排除的文件模式，相当于全局的 `.gitignore`。
7. logs
   - 这个目录包含所有引用（refs）的日志文件，记录了每一个引用（分支、标签等）的变化历史。
8. objects
   - 这个目录存储所有的数据对象，包括提交对象（commit）、树对象（tree）和数据对象（blob）。所有文件历史记录都存储在这个目录中的子目录和文件里。
9. refs
   - 这个目录包含所有的引用（分支、标签等），结构通常是 `heads`（分支）、`tags`（标签）和 `remotes`（远程分支）。

# 3 说说 Git 常用的命令有哪些？
1. 配置
   - git config [--global] user.name "[name]"
   - git config [--global] user.email "[email address]"
2. 启动
   - git init [project-name]
   - git clone url
3. 日常操作
   - git init
   - git add .
   - git add <具体某个文件路径+全名> 提交某些文件到缓存区
   - git diff 查看 add 内容
   - git diff --staged 查看 commit 内容
   - git status
   - git pull <远程仓库名> <远程分支名> 拉取远程仓库的分支与本地当前分支合并
   - git pull <远程仓库名> <远程分支名>:<本地分支名> 拉取远程仓库的分支与本地某个分支合并
   - git commit -m "<注释>"
   - git commit -v 提交时显示所有 diff 信息
   - **git commit --amend [file1] [file2]** 重做上一次 commit，并包括指定文件的新变化
4. 分支操作
   - git branch 查看本地所有分支
   - git branch -r 查看远程所有分支
   - git branch -a 查看本地和远程所有分支
   - git merge <分支名>
   - **git merge --abort** 合并分支出现冲突时，取消合并，一切回到合并前的状态
   - git branch <新分支名> 基于当前分支，新建一个分支，不会切换到新分支上
   - git checkout -b <新分支名> 基于当前分支，新建一个分支，并切换到新分支上
   - git checkout --orphan <新分支名> 新建一个空分支（创建的分支并不是基于任何现有分支的最新提交，而是一个完全没有历史记录的新分支。这意味着新分支没有任何父提交，也没有任何与现有分支共享的提交。**你只会有工作区中的文件，并且所有文件都会显示为未跟踪状态**）
   - git branch -D <分支名> 删除本地某个分支
   - git push <远程仓库>:<分支名> 删除远程某个分支（git push <远程仓库> --delete <分支名>也可以）
   - **git branch <新分支名称> <提交ID>** 从特定的提交点开始创建一个新分支
   - git checkout <分支名>
   - git checkout <远程库名>/<分支名> 切换到线上某个分支
5. 远程同步
   - git fetch [remote] 下载远程仓库的所有变动
   - git remote -v 显示所有远程仓库
   - git pull [remote] [branch] 拉取远程仓库分支与当前本地分支合并
   - git fetch 获取线上最新版信息记录，不合并
   - git push [remote] [branch] 上传本地分支到远程仓库
   - git push [remote] --force 强行推动当前分支到远程仓库，即使有冲突
   - git push [remote] --all 推动所有分支到远程仓库
6. 撤销
   - git checkout [file] 恢复暂存区的指定文件到工作区
   - **git checkout [commit] [file]** 恢复某个 commit 的指定文件到暂存区和工作区
   - git chekcout . 恢复暂存区的所有文件到工作区
   - git reset [commit] 重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
   - **git reset --hard** 重置暂存区和工作区，与上一次 commit 保持一致
   - git reset [file] 重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
   - **git revert [commit] 撤销特定的提交**（通过创建一个新的提交来撤销指定提交的更改，因此不会破坏提交历史。这使得 git revert 特别适合在公共分支上使用，因为它不会重写历史记录）
7. 存储
   - git stash 将当前工作目录中的未提交更改（包括暂存区和工作区的更改）保存到一个堆栈中，并清理工作目录，使其恢复到干净的状态。**（自己习惯用 git stash save -u）**
   - git stash pop 取出储藏中最后存入的工作状态进行恢复，会删除储藏
   - git stash list
   - git stash apply <储藏的名称> 取出储藏对应的工作状态进行恢复，不会删除储藏
   - git stash clear
   - git stash drop <储藏的名称> 删除对应的某个储藏
commit 规范：
- feat
- fix
- refactor（代码重构）
- docs（文档修改）
- style（代码格式修改，注意不是 css 修改）
- test（测试用例修改）
- chore（其他修改，例如构建流程、依赖管理）

reset 和 revert 的区别：
- reset **（慎用）**：真实硬性回滚，目标版本后面的提交记录全部丢失了
- revert：同样回滚，这个回滚操作相当于一个提交，目标版本后面的提交疾苦也全部都有

# 4 说说 Git 中 HEAD、工作树和索引之间的区别？
## 4.1 HEAD
当我们切换分支时，HEAD 指针通常指向我们所在的分支；当我们在某个分支上创建新的提交时，分支指针总是会指向当前分支的最新提交。

## 4.2 工作树和索引
在 Git 管理下，大家实际操作的目录被成为工作树，也就是工作区域。

在数据库和工作树之间有索引，索引是为了向数据库提交做准备的区域，也被成为暂存区域。

## 4.3 区别
![image](https://github.com/user-attachments/assets/293b55ab-c25a-4886-aa81-4d3ea5c8eaab)

# 5 说说 git 发生冲突的场景？如何解决？
## 5.1 是什么
出现冲突的场景：多个分支修改了同一个文件（任何地方）或多个分支修改了同一个文件的名称。

![image](https://github.com/user-attachments/assets/d2dc878f-dae4-49c5-a283-1a22d6198bac)

## 5.2 分析
分析特定场景下执行 merge 时，是否会发生冲突。

![image](https://github.com/user-attachments/assets/8533bad2-cbe2-4b8c-b14d-b6ef0e4861a3)

## 5.3 总结
![image](https://github.com/user-attachments/assets/faa28530-93ec-4b1c-8697-2ce0c6421a63)

# 6 说说 Git 中 fork、clone、branch 这三个概念，有什么区别？
## 6.1 是什么

## 6.2 如何使用
整体流程见下：
![image](https://github.com/user-attachments/assets/82c97f90-b575-4081-b518-7c6b5c15046c)

## 6.3 区别
![image](https://github.com/user-attachments/assets/f8bb8f36-5147-4ebf-8bf1-8f283ffd8ca6)

# 7 说说你对 git pull 和 git fetch 的理解？有什么区别？
## 7.1 是什么
![image](https://github.com/user-attachments/assets/97873ae8-085c-440d-90e1-d493371a8b62)

![image](https://github.com/user-attachments/assets/73cbbdf7-42e8-4576-847e-11b795cc7175)

## 7.2 使用
