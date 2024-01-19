# Q & A

## 如何删除 commit 里 merge 的提交内容

1. git rebase origin 提交分支
2. git add .
3. git commit --amend --no-edit
4. git push origin 提交分支 -f

## 重启CI

1. git reset
2. git commit --amend --no-edit
3. git push origin 提交分支 -f

## 删除远程和本地分支

- __删除本地分支__

>git branch -D 删除分支名

- __删除远程分支__

>git push origin --delete 远程分支名

## Untrack files 不想 commit

git add .
git reset --hard HEAD

## 冲突（无法在线解决）

1. 切到main拉库
2. 切到分支：git rebase main
3. 修改冲突文件
4. git add .
5. git push origin 分支名 -f

## 撤销commit

git log
git reset --hard 版本号
git add .
git commit --amend --no-edit
git push origin 提交分支 -f

note!!!
前置有git rebase main
git rebase --skip
git revert HEAD
但不确定是否有影响

## 无法连接到服务器

Failed to connect to github.com port 443 after 21057 ms: Couldn't connect to server

- 方法一（不是特别灵）
git config --global --unset http.proxy
git config --global --unset https.proxy

- 方法二
设置 --> 网络和Internet --> 代理 --> 手动设置代理 --> 端口 -->找到本机端口（端口号若为四位数，则需在最前方补0）

git config --global http.proxy http://127.0.0.1:xxxxx
git config --global https.proxy http://127.0.0.1:xxxxx
