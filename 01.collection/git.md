# Git Remark

经常用到Git，但是很多命令记不住，将其整理于此。（大量摘自网络）

一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要要记住60~100个命令。

![git](https://cdn.fkwar.com/869f4282-8b84-4bd9-a582-37193acd6d23.png)

![git](https://cdn.fkwar.com/05D5C85E-4DF0-4EE4-A33E-42E8788380F7.png)

## 下面整理的 Git 命令清单。几个专业名词的译名如下。

* Workspace：工作区
* Index / Stage：暂存区
* Repository：仓库区（本地仓库）
* Remote：远程仓库

## 新建版本仓库

```bash
# 在当前目录新建一个Git代码库
git init

# 新建一个目录，将其初始化为Git代码库
git init [project-name]

# 下载一个项目和它的整个代码历史, -o 给远程仓库起名:faker,默认origin
git clone [-o faker] [url]
```

## 配置

Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```bash
# 显示当前的Git配置
git config --list

# 编辑Git配置文件
git config -e [--global]

# 设置提交代码时的用户信息
git config [--global] user.name "[name]"
git config [--global] user.email "[email address]"

# 设置大小写敏感（windows不区分大小写的解决办法）
git config core.ignorecase  false
```

## 增加/删除文件

```bash
# 添加指定文件到暂存区
git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
git add [dir]

# 添加当前目录的所有文件到暂存区
git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
git add -p

# 删除工作区文件，并且将这次删除放入暂存区
git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
git mv [file-original] [file-renamed]
```

## 代码提交

```bash
# 提交暂存区到仓库区
git commit -m [message]

# 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

# 提交时显示所有diff信息
git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
git commit --amend [file1] [file2] ...
```

## 分支

```bash
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 列出所有本地分支和远程分支
git branch -a

# 列出所有本地分支，并展示没有分支最后一次提交的信息
git branch -v

# 列出所有本地分支，并展示没有分支最后一次提交的信息和远程分支的追踪情况
git branch -vv

# 列出所有已经合并到当前分支的分支
git branch --merged

# 列出所有还没有合并到当前分支的分支
git branch --no-merged

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 新建一个与远程分支同名的分支，并切换到该分支
git checkout --track [branch-name]

# 新建一个分支，指向指定commit
git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

# 切换到上一个分支
git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream-to=[remote-branch]
git branch --set-upstream [branch] [remote-branch] # 已被弃用

# 合并指定分支到当前分支
git merge [branch]

# 中断此次合并（你可能不想处理冲突）
git merge --abort

# 选择一个commit，合并进当前分支
git cherry-pick [commit]

# 删除分支
git branch -d [branch-name]

#新增远程分支 远程分支需先在本地创建,再进行推送
git push origin [branch-name]

# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```

## 标签

```bash
# 列出所有tag
git tag

# 新建一个tag在当前commit
git tag [tag]

# 新建一个tag在指定commit
git tag [tag] [commit]

# 删除本地tag
git tag -d [tag]

# 删除远程tag
git push origin :refs/tags/[tagName]

# 查看tag信息
git show [tag]

# 提交指定tag
git push [remote] [tag]

# 提交所有tag
git push [remote] --tags

# 新建一个分支，指向某个tag
git checkout -b [branch] [tag]
```

## 查看信息/搜索

```bash
# 显示有变更的文件
git status [-sb] #s:short,给一个短格式的展示，b:展示当前分支

# 显示当前分支的版本历史
git log

# 显示commit历史，以及每次commit发生变更的文件
git log --stat

# 搜索提交历史，根据关键词
git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
git log --follow [file]
git whatchanged [file]

# 显示指定文件相关的每一次diff
git log -p [file]

# 显示过去5次提交
git log -5 --pretty --oneline

# 图形化显示所有分支
git log --oneline --graph --all

# 显示在分支2而不在分支1中的提交
git log [分支1]..[分支2]
git log ^[分支1] [分支2]
git log [分支2] --not [分支1]

# 显示两个分支不同时包含的提交
git log [分支1]...[分支2]

# 显示所有提交过的用户，按提交次数排序
git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
git blame [file]

# 显示暂存区和工作区的差异
git diff

# 显示暂存区和上一个commit的差异
git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
git diff HEAD

# 显示两次提交之间的差异
git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
git show [commit]

# 显示某次提交发生变化的文件
git show --name-only [commit]

# 显示某次提交时，某个文件的内容
git show [commit]:[filename]

# 显示当前分支的最近几次提交
git reflog

# 搜索你工作目录的文件，输出匹配行号
git grep -n [关键字]

# 搜索你工作目录的文件，输出每个文件包含多少个匹配
git grep --count [关键字]

# 优化阅读
git grep --break --heading [关键字]

# 查询iCheck这个字符串那次提交的
git log -SiCheck --oneline

# 查询git_deflate_bound函数每一次的变更
git log -L :git_deflate_bound:zlib.c
```

## 远程同步

```bash
# 下载远程仓库的所有变动 [shortname] 为远程仓库的shortname, 如origin,为空时:默认origin
git fetch [shortname]

# 显示所有远程仓库
git remote -v

#显式地获得远程引用的完整列表 [shortname] 为远程仓库的shortname, 如origin,为空时:默认origin
git ls-remote [shortname]

# 显示某个远程仓库的信息 [remote] 为远程仓库的shortname, 如origin
git remote show [shortname]

# 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

# 重命名一个远程仓库（shortname）
git remote rename [旧仓库名] [新仓库名]

# 删除一个远程链接
git remote rm [shortname] [url]
git remote remove [shortname] [url]

# 修改远程仓库地址
git remote set-url [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

# 上传本地当前分支到远程仓库
git push [remote]

# 上传本地指定分支到远程仓库
git push [remote] [branch]

# 推送所有分支到远程仓库
git push [remote] --all

# 强行推送当前分支到远程仓库，即使有冲突
### WARNING ###
git push [remote] --force
git push -f [origin] [master]

# 强制获取并覆盖本地(丢掉本地仓库)
### WARNING ###
git fetch --all
git reset --hard original/[master/beta/dev]
git pull
```

## 常规操作

```bash
# Git 单独某个仓库设置
git config --local user.name "zhaopan"
git config --local user.email "zhaopan@gmail.com"

# 若创建新版本库
git clone git@github.com:zhaopan/codesnippet.git
cd ax
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

# 若已存在的文件夹或 Git 仓库
cd git_existing_folder
git init
git remote add origin git@github.com:zhaopan/codesnippet.git
# or
git remote set-url origin git@github.com:zhaopan/codesnippet.git
git add .
git commit
git push -u origin master
```

## 撤销

```bash
# 恢复暂存区的指定文件到工作区
git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
git checkout .

#只会保留源码（工作区），回退commit(本地仓库)与index（暂存区）到某个版本
git reset <commit_id>   #默认为 --mixed模式
git reset --mixed <commit_id>

#保留源码（工作区）和index（暂存区），只回退commit（本地仓库）到某个版本
git reset --soft <commit_id>

#源码（工作区）、commit（本地仓库）与index（暂存区）都回退到某个版本
git reset --hard <commit_id>

# 恢复到最后一次提交的状态
git reset --hard HEAD

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]

# 将工作区和暂存区的代码全都存储起来了
git stash [save]

# 只保存工作区，不存储暂存区
git stash --keep-index

# 存储工作区、暂存区和未跟踪文件
git stash -u
git stash --include-untracked

# 不存储所有改动的东西，但会交互式的提示那些改动想要被储藏、哪些改动需要保存在工作目录中
git stash --patch

# 不指定名字，Git认为指定最近的储藏，将存储的代码（工作区和暂存区）都应用到工作区
git stash apply [stash@{2}]

# 存储的工作区和暂存区的代码应用到工作区和暂存区
git stash apply [stash@{2}] --index

# 将存储的代码（工作区和暂存区）都应用到工作区，并从栈上扔掉他
git stash pop

# 删除stash@{2}的存储
git stash drop [stash@{2}]

# 获取储藏的列表
git stash list

# 移除工作目录中所有未跟踪的文件及口口那个的子目录，不会移除.gitiignore忽略的文件
git clean -f -d
```

## sparse-checkout

```bash
mkdir models # 创建一个与要clone的仓库同名或不同命的目录
cd models
git init #初始化
git remote add origin https://github.com/zhaopan/codesnippet.git # 增加远端的仓库地址
git config core.sparsecheckout true # 设置Sparse Checkout 为true
echo docker >> .git/info/sparse-checkout # 将要部分clone的目录相对根目录的路径写入配置文件
git pull origin master #pull下来代码
# 如果只想保留最新的文件而不要历史版本的文件，上例最后一行可以用git pull --dpeth 1命令，即“浅克隆”：
git pull --depth 1 origin master

# 如果需要添加目录，就增加sparse-checkout的配置，再checkout master
echo application >> .git/info/sparse-checkout
git checkout master

# 后来上面方法遇到错误

##error: Sparse checkout leaves no entry on working directory
又找到另一种方法如下。最后发现，如果在shell里执行，sparse-checkout 里的路径需要最后加*，但是如果是git-prompt,则可以不需要最后的/*.

git clone -n https://github.com/tensorflow/models
cd tensorflow
git config core.sparsecheckout true
echo official/resnet/* >> .git/info/sparse-checkout
git checkout master
```

## git 迁移

```bash
# 旧项目
http://old.git.com/project.git

# 新项目
http://new.git.com/project.git

git clone http://old.git.com/project.git
cd project
git remote set-url origin http://new.git.com/project.git

# 期间可能存在分支
git branch
git checkout dev

# 迁移命令
git push -u origin master # 若new 中不存在分支
git push --force origin master # 若new 中存在分支，则强制推送
```

## 常规命令

```bash
# 生成一个可供发布的压缩包
git archive

# 本地保存密码
git config --global credential.helper store

# git push 超限
git config --global http.postBuffer 524288000

# 删除git项目下的空文件夹
git clean -fd

# 拉取最新版本
git reset --hard
git pull

# 覆盖本地仓库
git fetch origin master

# 例如，我要拉取远端其他小伙伴提交的新分支 test
git fetch
git checkout -b test origin/test

# 提交到本地仓库
git commit -am 'commit remark'

# 推送至远程仓库
git push origin master

# 分支推送，若远程不存在分支则在远程创建新分支
git push origin 本地分支名:远程分支名

# 查看用户名和邮箱地址
git config user.name
git config user.email

# 修改用户名和邮箱地址
git config --global user.name "xxx"
git config --global user.email "xxx@gmail.com"
```

## 清理仓库

```bash
# 参考资料
# https://www.cnblogs.com/shines77/p/3460274.html
# https://blog.csdn.net/weixin_45115705/article/details/90604963

# 从你的资料库中清除文件
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch path-to-your-remove-file' --prune-empty --tag-name-filter cat -- --all

# exp:
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch *.exe' --prune-empty --tag-name-filter cat -- --all


#begin 若是移除大文件或指定文件--------------

git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -3
#16779d71545f8b76faf02afffe5544ca87a4aaac blob 1102745 1102346 8459682
#68f450adbce465995f52796f05956f4f1fe79429 blob 2081189 2081811 5111192
#d0781e7d125599010f4885fa95802a1d7018cd44 blob 278367052 278045657 10601748
git rev-list --objects --all | grep d0781e7d125599010f4885fa95802a1d7018cd44
#d0781e7d125599010f4885fa95802a1d7018cd44 nginx/nginx.exe
git log --pretty=oneline --branches -- nginx/nginx.exe
git filter-branch --index-filter 'git rm --cached --ignore-unmatch nginx/nginx.exe' -- --all

#--------------若是移除大文件或指定文件 end

# 推送我们修改后的repo
git push origin master --force

# 清理和回收空间
rm -rf .git/refs/original/
rm -rf .git/logs/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
git push --force
```
