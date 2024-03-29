# Git

## Git操作
### 安装Git
* git官网下载安装[git](https://git-scm.com/downloads)     
* 检测`git`是否安装成功
  1. windows系统的桌面右键查看有以下两个命令
     Git GUI Here    
     Git Bash Here
  2. 终端Terminal中输入以下命令查看是否出现git版本号。(windows/Mac通用)
     git --version        
* 取消ssl证书命令：
```shell
git config --global http.sslVerify false  
```

### 文件基础操作
1. 一个简单的操作步骤：
```git
$ git init 
$ git add .
$ git commit
```
* git init      -初始化仓库    
* git add .     -添加文件到暂存区     
* git commit    -将暂存区内容添加到仓库中  
2. 配置用户名和邮箱
   * 用户名： git config --global user.name `用户名`     
   * 邮箱： git config --global user.email `邮箱`     
   * 查看是否配置成功： git config --global --list   
3. 改动文件名称： git mv 改动前文件名 改动后文件名
4. 移动文件：git mv 文件名 文件夹  
5. 还原文件：git checkout -- 文件夹/文件名   
   丢弃工作区的修改：git restore 文件名
   退回版本：git reset --hard HEAD^
            git reset --hard `id`  
6. 推送至远程仓库：git push origin master  
7. 添加标签名： git tag `标签名` `id`
   删除标签名： git tag -d `标签名`   
   推送至远程仓库： git push origin `tag名`   

### 分支基础操作
1. 创建分支名：git branch `分支名`   
   创建并切换分支：git checkout -b `分支名`    
2. 切换分支：git checkout `分支名`    
3. 删除分支：git branch -d `分支名`    
4. 合并分支：git merge `分支名`  
   解决冲突：git merge -abort `分支名`  

## Git主要知识点

1. 操作暂存区快照
2. 提交和日志
3. 分支管理 
4. 文件对比，冲突解决
5. 远程仓库同步

## Git 配置

```shell
git config --global user.name "storm"
git config --global user.email "stormzhang.dev@gmail.com"
git config --global color.ui true
git config --global alias.co checkout  # 别名
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --' #取消暂存！
git config --global core.editor "vim"  # 设置Editor使用vim
git config --global core.quotepath false # 设置显示中文文件名
```

用户的 git 配置文件~/.gitconfig

## Git 常用命令

#### 查看、添加、提交、删除、找回，重置修改文件

```shell
git help <command>  # 显示command的help
git show            # 显示某次提交的内容
git show $id

git co  -- <file>   # 抛弃工作区修改
git co  .           # 抛弃工作区修改

git add <file>      # 将工作文件修改提交到本地暂存区
git add .           # 将所有修改过的工作文件提交暂存区
git add -u          # 只将已跟踪的文件添加，不添加新文件

git rm <file>       # 从版本库中删除文件
git rm <file> --cached  # 从版本库中删除文件，但不删除文件

git reset <file>    # 从暂存区恢复到工作文件
git reset -- .      # 从暂存区恢复到工作文件
git reset --hard    # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git ci <file>
git ci .
git ci -a           # 将git add, git rm和git ci等操作都合并在一起做
git ci -am "some comments"
git ci --amend      # 修改最后一次提交记录

git revert <$id>    # 恢复某次提交的状态，恢复动作本身也创建了一次提交对象
git revert HEAD     # 恢复最后一次提交的状态
```

#### 查看文件 diff

```shell
git diff <file>     # 比较当前文件和暂存区文件差异
git diff
git diff <$id1> <$id2>   # 比较两次提交之间的差异
git diff <branch1>..<branch2> # 在两个分支之间比较
git diff --staged   # 比较暂存区和版本库差异
git diff --cached   # 比较暂存区和版本库差异
git diff --stat     # 仅仅比较统计信息
```

#### 查看提交记录

```shell
git log
git log <file>      # 查看该文件每次提交记录
git log -p <file>   # 查看每次详细修改内容的diff
git log -p -2       # 查看最近两次详细
```

## Git 本地分支管理

#### 查看、切换、创建和删除分支

```shell
git br -r           # 查看远程分支
git br <new_branch> # 创建新的分支
git br -v           # 查看各个分支最后提交信息
git br --merged     # 查看已经被合并到当前分支的分支
git br --no-merged  # 查看尚未被合并到当前分支的分支

git co <branch>     # 切换到某个分支
git co -b <new_branch> # 创建新的分支，并且切换过去
git co -b <new_branch> <branch>  # 基于branch创建新的new_branch

git co $id          # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
git co $id -b <new_branch>  # 把某次历史提交记录checkout出来，创建成一个分支

git br -d <branch>  # 删除某个分支
git br -D <branch>  # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)
```

#### 分支合并和 rebase

```shell
git merge <branch>               # 将branch分支合并到当前分支
git merge origin/master --no-ff  # 不要Fast-Foward合并，这样可以生成merge提交

git rebase master <branch>       # 将master rebase到branch，相当于：
git co <branch> && git rebase master && git co master && git merge <branch>
```

#### Git 补丁管理(方便在多台机器上开发同步时用)

```shell
git diff > ../sync.patch         # 生成补丁
git apply ../sync.patch          # 打补丁
git apply --check ../sync.patch  # 测试补丁能否成功
```

#### Git 暂存管理

```shell
git stash                        # 暂存
git stash list                   # 列所有stash
git stash apply                  # 恢复暂存的内容
git stash drop                   # 删除暂存区
git stash clear
```

#### Git 远程分支管理

```shell
git pull                         # 抓取远程仓库所有分支更新并合并到本地
git pull --no-ff                 # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
git fetch origin                 # 抓取远程仓库更新
git merge origin/master          # 将远程主分支合并到本地当前分支
git co --track origin/branch     # 跟踪某个远程分支创建相应的本地分支
git co -b <local_branch> origin/<remote_branch>  # 基于远程分支创建本地分支，功能同上

git push                         # push所有分支
git push origin master           # 将本地主分支推到远程主分支
git push -u origin master        # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
git push origin <local_branch>   # 创建远程分支， origin是远程仓库名
git push origin <local_branch>:<remote_branch>  # 创建远程分支
git push origin :<remote_branch>  #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
```

#### Git 远程仓库管理

```shell
git remote -v                    # 查看远程服务器地址和仓库名称
git remote show origin           # 查看远程服务器仓库状态
git remote add origin git@github:stormzhang/demo.git         # 添加远程仓库地址
git remote set-url origin git@github.com:stormzhang/demo.git # 设置远程仓库地址(用于修改远程仓库地址
```

#### 创建远程仓库

```shell
git clone --bare robbin_site robbin_site.git # 用带版本的项目创建纯版本仓库
scp -r my_project.git git@git.csdn.net:~ # 将纯仓库上传到服务器上

mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库
git remote add origin git@github.com:robbin/robbin_site.git # 设置远程仓库地址
git push -u origin master # 客户端首次提交
git push -u origin develop # 首次将本地 develop 分支提交到远程 develop 分支，并且 track

git remote set-head origin master # 设置远程仓库的 HEAD 指向 master 分支

也可以命令设置跟踪远程库和本地库

git branch --set-upstream master origin/master
git branch --set-upstream develop origin/develop

```

#### 创建标签

```shell
git tag -a v1.4 -m "my version 1.4" # 创建带有附加信息的标签
git tag                             # 列出所有标签
git show v1.4                       # 查看具体标签
git push origin v1.4                # 推送标签到远程仓库
git tag -d v1.4                     # 删除本地标签
git push origin :refs/tags/v1.4     # 删除远程标签
```

## 参考   
[菜鸟：git教程](https://www.runoob.com/git/git-tutorial.html)        
[掘金：Git - 在版本之间切换自如](https://juejin.cn/post/6844903876642996238)       
[问题：Failed to connect to github.com port 443:connection timed out](https://blog.csdn.net/Hodors/article/details/103226958)     