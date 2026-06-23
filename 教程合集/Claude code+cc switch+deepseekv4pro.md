# Claude code+cc switch+deepseekv4pro

## 安装准备

- windows for git
- cc swith protable (D:\Deepseek_Agent\cc_switch)
- deepseek API key


## 开始安装

1. 创建Claude运行路径

- 新建文件夹，为Claude后续项目运行文件。此处为：`D:\Deepseek_Agent\`
- 同时在该文件夹下新建两个子文件夹 `D:\Deepseek_Agent\cc-switch\` 和 `D:\Deepseek_Agent\.claude_agent_config`

2. 下载并配置cc-switch
- 下载 [cc-switch-windows-portable.zip](https://github.com/farion1231/cc-switch/releases)
- 解压，把所有文件拷贝到 `D:\Deepseek_Agent\cc-switch\`
- 双击运行 `cc-switch.exe`

>在软件页面添加DeepSeek供应商配置

- 应用选择 __Claude__
- API Key填入已有DeepSeek密钥 
- 其余保持原样

3. Bash配置Claude

整体安装都在Bash，所以需要保证已装git

- 在 `D:\Deepseek_Agent` 文件夹空白处右键，选择 __Git Bash Here__。此时会弹出一个纯黑色的 Git Bash 命令行窗口。

- 拷贝如下命令，保证专属的本地对话缓存在D盘
```
export CLAUDE_CONFIG_DIR="/d/Deepseek_Agent/.claude_agent_config"
```

- 配置环境 (需要科学上网，7890是Clash系列代理的端口，其余端口可在win里搜索“代理”查询)

- 分别在bash里运行如下两行代码

`export http_proxy="http://127.0.0.1:7890"`

`export https_proxy="http://127.0.0.1:7890"`

- 安装Claude

`powershell -Command "irm https://claude.ai/install.ps1 | iex"`

可能会有一个报错，是set upnotes，是配置环境的。
直接在bash里输入如下命令，注意，__users__ 后面的 __mia__ 需要换成你实际的用户名。

`setx PATH "$env:PATH;C:\Users\mia\.local\bin"`

## 使用Claude

- 在Bash里输入 `Claude`会出现Claude
初始配置一直 `Enter`确认即可

- 如果后续有迁移资料需求，可以通过资源管理器，输入 `%USERPROFILE%` ，找到 `.claude`文件夹复制。再输入 `%LOCALAPPDATA%` 复制 `Claude`文件夹。

## Claude命令

### 控制模型
/model

### 控制记忆 
/context 查看上下文

/clear 清空上下文

/reset 重置会话

/add <file> 加入上下文

### 控制项目
/init 初始化agent项目

/open 打开工作目录

### 控制行为
/system system prompt（高级）

/debug debug 模式

### 控制执行
/run <cmd> 执行 shell 命令

/shell 进入 shell 模式

/tools 查看 tool 能力