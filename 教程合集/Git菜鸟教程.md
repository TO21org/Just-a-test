# <center>Git摸索</center>

## 安装配置
<span style="background-color: #66CDAA">git config</mark>

主目录一般为：C:\Documents and Settings\$USER

### 用户信息
配置个人的用户名称和电子邮件
<table><tr><td bgcolor=BBC8E6>
$ git config --global user.name "runoob" 

$ git config --global user.email test@runoob.com
</td></tr></table> 

`--global`是全局模式，更改用户主目录下的配置文件

去掉这一选项则可以使用特定项目中的名字和邮箱

### 查阅当前状态
`git status`

### 文本编辑器
默认为Vi或Vim，如需更改为Emacs，可以按照如下设置
<span style="background-color: #ADD8E6">
$ git config --global core.editor emacs
</mark>
### 差异分析工具
如下为改用vimdiff命令
<span style="background-color: #ADD8E6">
$ git config --global merge.tool vimdiff
</mark>

其余工具：kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和 opendiff

### 查看配置信息
<span style="background-color: #ADD8E6">
$ git config --list
</mark>

显示当前git配置信息

<span style="background-color: #ADD8E6">
$ git config -e
</mark>

针对当前仓库

<span style="background-color: #ADD8E6">
$ git config -e --global
</mark>

针对系统上所有的库

#### 查阅某个环境变量的设定
<span style="background-color: #ADD8E6">
$ git config user.name
</mark>

## Git工作区暂存区和版本库
工作区：电脑里可以看到的目录
暂存区：stage或index（可以视为副本区，一切操作都是先基于副本的）
版本库：包括index和master

![基本概念](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

`git add` 工作区修改或新增文件到暂存区
`git commit` 提交暂存区到本地仓库 
`git reset HEAD` 暂存区的目录树会由master分支指向的目录树替代
`git rm --cached <file>` 直接从暂存区删除文件，工作区则不做出改变
`git checkout .`或者`git checkout -- <file>`会用暂存区全部或指定文件替换工作区的内容（慎用）
`git checkout HEAD .`或者`git checkout HEAD <file>` 会用HEAD指向master分支的内容替换缓存区及工作区的内容（慎用）

总而言之，菜鸟别碰checkout

## Git创建仓库
初始化当前目录（仅用于空目录创建新仓库）
<span style="background-color: #E0EBAF">
$ git init
</mark>

生成一个新的.git目录

使用指定目录作为Git仓库
<span style="background-color: #E0EBAF">
$ git init newrepo
</mark>

提交目录下`以.c结尾`以及`README`文件提交到仓库中

<table><tr><td bgcolor=BCE6B3>

$ git add *.c

$ git add README 

$ git commit -m "初始化项目版本"

</td></tr></table> 

### git clone: 从Git仓库中拷贝项目

<span style="background-color: #E0EBAF">
$ git clone <repo>
</mark>

克隆仓库`repo（Git仓库）`


<span style="background-color: #E0EBAF">
$ git clone <repo> <directory>
</mark>

克隆到指定仓库`directory（本地目录）`

<span style="background-color: #E0EBAF">
$ git clone git://github.com/schacon/grit.git
</mark>
    
克隆Ruby 语言的 Git 代码仓库 Grit（就是复制项目代码）

<span style="background-color: #E0EBAF">
$ git clone git://github.com/schacon/grit.git mygrit
</mark>

定义新建项目目录名称

## Git基本操作
常用六个命令

| [git clone](#git-clone-从git仓库中拷贝项目) | [git add](#git-add)  | [git commit](#git-commits)  |
| ---- | --- | --- |
| [git push](#git-push)     | [git pull](#git-pull)    | [git checkout](#git-checkout)  |

![相互关联](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

### git add

添加一个或多个文件到暂存区：
`git add [file1] [file2] ...`

想删除暂存区文件
`git rm --cached <file>` 

添加指定目录到暂存区，包括子目录：
`git add [dir]`

添加当前目录下的所有文件到暂存区： 
`git add .`

文件修改后，我们一般都需要进行 git add 操作，从而保存历史版本。（这个没懂）

### git commit

提交暂存区到本地仓库中:
`git commit -m [message]` message这里可以指备注信息

提交暂存区的指定文件到仓库区：
`$ git commit [file1] [file2] ... -m [message]`

-a 参数设置修改文件后不需要执行 git add 命令，直接提交
`$ git commit -a`

[菜鸟教程](https://www.runoob.com/git/git-commit.html)看着看着就飞升了，看不懂了。

### git checkout

分支切换
`git checkout <branch-name>`

切换到主分支
`git checkout master `

创建并切换到新分支
`git checkout -b <new-branch-name>`

切换到前一分支
`git checkout -`

将指定文件恢复到最新的提交状态，丢弃所有未提交的更改
`git checkout -- <file>`

切换到标签
`git checkout tags/<tag-name>`

### git push
用于从将本地的分支版本上传到远程并合并
`git push <远程主机名> <本地分支名>:<远程分支名>`
$ git push origin master:master

若本地分支名与远程分支名相同
`git push <远程主机名> <本地分支名>`
$ git push origin master

若本地版本与远程版本有差异须强制推送
`git push --force origin master`

删除 origin 主机的 master 分支
`git push origin --delete master`

### git pull
远程获取代码并合并本地的版本
`git pull <远程主机名> <远程分支名>:<本地分支名>`

将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并
`git pull origin master:brantest`

远程分支是与当前分支合并
`git pull origin master`

[其余操作](https://www.runoob.com/git/git-basic-operations.html)

## Git分支管理

创建分支命令
`git branch (branchname)`

切换分支
`git checkout (branchname)`

合并分支
`git merge `

列出分支
`git branch`

删除分支
`git branch -d (branchname)`


## Git查看提交历史

查看历史提交记录
`git log`

列表形式查看指定文件修改记录
`git blame<file>-`

### git log

<span style="background-color: #E0EBAF">
git log [选项] [分支名/提交哈希]
</mark>

[选项如下]
```
    -p：显示提交的补丁（具体更改内容）。
    --oneline：以简洁的一行格式显示提交信息。
    --graph：以图形化方式显示分支和合并历史。
    --decorate：显示分支和标签指向的提交。
    --author=<作者>：只显示特定作者的提交。
    --since=<时间>：只显示指定时间之后的提交。
    --until=<时间>：只显示指定时间之前的提交。
    --grep=<模式>：只显示包含指定模式的提交消息。
    --no-merges：不显示合并提交。
    --stat：显示简略统计信息，包括修改的文件和行数。
    --abbrev-commit：使用短提交哈希值。
    --pretty=<格式>：使用自定义的提交信息显示格式。
```

[更多信息](https://git-scm.com/docs/git-log)
git log --help


### git blame

<span style="background-color: #E0EBAF">
git blame [选项] <文件路径>
</mark>

[选项如下]
```
    -L <起始行号>,<结束行号>：只显示指定行号范围内的代码注释。
    -C：对于重命名或拷贝的代码行，也进行代码行溯源。
    -M：对于移动的代码行，也进行代码行溯源。
    -C -C 或 -M -M：对于较多改动的代码行，进行更进一步的溯源。
    --show-stats：显示包含每个作者的行数统计信息。
```
更多信息：git blame --help

## Git 标签

-a 带注解的标签
`$ git tag -a v1.0 `

指定标签信息命令
`git tag -a <tagname> -m "runoob.com标签"`

PGP签名标签命令
`git tag -s <tagname> -m "runoob.com标签"`


## Git远程仓库（Github）

[详细教程](https://www.runoob.com/git/git-remote-repo.html)

添加远程仓库
`git remote add [shortname] [url]`

查看当前配置有哪些远程仓库
`git remote`

### 提取远程仓库

从远程仓库下载新分支与数据
`git fetch`

从远端仓库提取数据并尝试合并到当前分支
`git merge`

![](https://www.runoob.com/wp-content/uploads/2015/03/main-qimg-00a6b5a8ec82400657444504c4d4d1a7.png)

### 推送到远程仓库

推送你的新分支与数据到某个远端仓库
`git push [alias] [branch]`

删除远程仓库
`git remote rm [别名]`

暂时完结，更多[参看](https://www.runoob.com/git/git-remote-repo.html)