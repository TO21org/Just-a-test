# <center>Git 简略版</center>

## 本地仓库命令操作

## 配置信息

配置个人的用户名称和电子邮件
<table><tr><td bgcolor=BBC8E6>
$ git config --global user.name yourname

$ git config --global user.email test@anything.com
</td></tr></table> 

`--global`是全局模式，更改用户主目录下的配置文件

去掉这一选项则可以使用特定项目中的名字和邮箱

## 初始化

在相应文件夹下打开命令窗口Git Bash Here
`git init`

## 查阅当前状态
`git status`

## 添加文件到暂存区

添加一个或多个文件
`git add [file1][file2]...`

添加所有txt文件
`git add *.txt`

添加当前目录下的所有文件到暂存区： 
`git add .`

## 删除暂存区文件
`git rm --cached [file]`

## 提交暂存区文件到本地仓库

`git commit -m [message]` message这里可以指备注信息

## 查看提交历史

`git log`

简洁一行显示
`git log --oneline`

## 删除文件（非指令）

手动删除文件夹内文件后

`git add shanchu.txt`
`git commit -m 删除也要提交`

1. 如果没有提交
`git restore shanchu.txt`可以恢复文件

2. 如果提交了
- 可以查看历史记录，进入删除前版本
`git log --oneline`
`git reset --hard 删除前版本号`
但是`reset`会使得历史记录丢失
`git reflog -v`可以查看所有记录

- 通过还原进入前版本
`git log --oneline`
`git revert 删除文件版本号`

## 创建分支

分支是基于提交的，也就是说，需要先有add 和 commit，之后才能建立分支
`git branch [分支名]`

查看分支数
`git branch -v`

## 切换分支
`git checkout [分支名]`

创建+切换分支
`git checkout -b [分支名]`

## 删除分支

`git branch -d [分支名]`

## 合并分支
先切换到主分支（A合并到B，那么就切换到B）
`git merge [A]`

如果有文件冲突情况，人工判断删除合并，再提交

## 添加标签

给某次提交添加标签
`git tag [标签名] <版本号>`

创建分支添加标签
`git checkout -b [分支名] <tag的名称>`

删除标签
`git tag -d [标签名]`


## 远程仓库命令操作

添加远程仓库
`git remote add [shortname] [url]`

![](https://notes.sjtu.edu.cn/uploads/upload_01992cf9da3f3abbc55480b19ef2192e.png)

俩参数从文件里的config找，shortname一般为origin
url如果有SSH需要设置密匙

`ssh-keygen -t rsa -C[url地址]`

文件在>用户>.ssh文件夹>id_rsa.pub

推送本地到远程
`git push origin`

从远程更新到本地
`git pull origin`
