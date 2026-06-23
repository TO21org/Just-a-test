# Git VScode 全过程 for win

## 前提条件
- 下载 VScode
- [下载 Git](https://blog.csdn.net/mukes/article/details/115693833)

请全程保持 VScode 和 Git 登录

## 配置git和SSH

1. 在VScode中打开终端，可随意新建文件。在终端中输入以下命令

```
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

2. 复制如下命令，检查配置是否成功
```
git config --global -l
```

### 生成SSH密钥

1. win+R 输入 cmd 打开终端，输入如下命令
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

2. 连续按三次回车

> 注意：后两次回车是为了保持密码空白，否则每次使用SSH都要输入密码，非常麻烦。

### 查找SSH

密钥在C:\Users\YourUserName\.ssh\id_ed25519

1. 使用记事本打开 pub文件，复制全部内容

2. 登录GitHub 
__点击右上角头像__ > __Settings__ > __SSH and GPG keys__ > __New SSH key__

>title随便填，比如 My Laptop

3. 粘贴复制的SSH内容，点击 __Add SSH key__ 。

### 测试SSH连接
在VScode终端中输入
```
ssh -T git@github.com
```
如果成功，则返回
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
……
```

## 克隆已有项目到本地
1. 在本地新建文件夹，比如 D:\Git，在VScode终端输入如下内容。
```
cd D:\Git
```
>切换到如上路径后，之后的克隆文件会下载到D:\Git文件夹下

2. 在github上进入要克隆的仓库，页面有一个 __绿色的code__ 标识，点击 __倒三角位置__，可以看到克隆有三个选项。选择 __HTTPS链接__ 即可。
粘贴链接，大概如下所示。
```
https://github.com/owner/repository.git
```

3. 在VScode终端输入如下命令克隆
```
git clone https://github.com/owner/repository.git
```

>如果出现代理错误，请在本机电脑设置中搜索 __代理服务器设置__
开启 __使用代理服务器__
该选项下有地址和端口，类似于
```
123.0.2.0 6790
```
在VScode终端输入
```
git config --global http.proxy http://123.0.2.0:6799
```
输入如下明确确认配置完成
```
git config --global -l
```

[参考链接](https://jishuzhan.net/article/1876450352648163330)


## 配置office-word-MCP

1. 在文件资源管理器地址栏粘贴如下内容
```
%USERPROFILE%\.claude
```

2. 在该文件夹下建立一个名为settings的json文件。在该json文件中复制如下内容
```
{
  "mcpServers": {
    "word-document-server": {
      "command": "python",
      "args": ["D:\\Git\\Office-Word-MCP-Server\\word_mcp_server.py"]
    }
  }
}
```
3. 在VScode终端运行python文件，验证是否成功。
```
python D:\\Git\\Office-Word-MCP-Server\\word_mcp_server.py
```
>出现如下字眼说明服务正常，按 __Ctrl+C__ 退出即可
```
Transport: stdio
Starting Word Document MCP Server with stdio transport...
Server running on stdio transport
```