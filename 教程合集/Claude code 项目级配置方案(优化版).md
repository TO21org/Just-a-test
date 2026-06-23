# Claude Code 项目级配置方案（优化版）

> 相比 cc-switch 方案，本方案通过**项目级配置隔离**，无需额外工具，直接切换项目自动切换模型。

---

## 核心原理

- **全局配置**：原生 Claude（OAuth 订阅）
- **项目级配置**：可选覆盖（如 DeepSeek）
- **工作流**：打开不同项目文件夹 → 自动切换模型

---

## 前置要求

- Windows 版本 git（默认路径 `C:\Program Files\Git\bin`，非默认安装需调整）
- Claude Code 已安装
- DeepSeek API Key（如需用 DeepSeek）
- Claude Pro/Max 订阅（原生模式用）

---

## 第一步：配置全局原生 Claude

### 1.1 清理全局设置

编辑 `C:\Users\{username}\.claude\settings.json`（用户名改成自己的）：

```json
{
  "model": "haiku"
}
```

> **说明**：
> - `env: {}` 空环保，自动 fallback 到 OAuth 订阅
> - `model: "haiku"` 为全局默认模型

### 1.2 恢复全局 MCP（office-word-MCP）

在 `C:\Users\{username}\.claude\` 目录下新建 `.mcp.json`：

```json
{
  "mcpServers": {
    "word-document-server": {
      "command": "python",
      "args": ["D:\\Git\\Office-Word-MCP-Server\\word_mcp_server.py"]
    }
  }
}
```

> 所有项目都可用该 MCP（除非项目级有覆盖）

---

## 第二步：修复 Git PATH（PowerShell 可用）

> 根据实际安装位置调整路径，默认为 `C:\Program Files\Git\bin`

在 PowerShell 运行一次（无需重复）：

```powershell
$gitPath = "C:\Program Files\Git\bin"  # 按实际路径修改
$currentPath = [Environment]::GetEnvironmentVariable("PATH", "User")
if ($currentPath -notlike "*$gitPath*") {
  [Environment]::SetEnvironmentVariable("PATH", "$currentPath;$gitPath", "User")
  Write-Host "Git path added. Restart PowerShell/Claude Code for effect."
}
```

之后重启 Claude Code，`git status` 就能用了。

---

## 第三步：配置 DeepSeek 项目（可选）

### 3.1 项目目录结构

```
D:\Deepseek_Agent\
├─ .claude\
│  ├─ settings.json       ← 项目级配置（DeepSeek env vars）
│  └─ settings.local.json ← 可选，权限/hooks
└─ （其他项目文件）
```

### 3.2 创建项目级 settings.json

在 `D:\Deepseek_Agent\.claude\settings.json` 中：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-your-deepseek-key",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1M]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL_NAME": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_SONNET_MODEL_NAME": "deepseek-v4-pro",
    "ANTHROPIC_MODEL": "deepseek-v4-pro"
  }
}
```

> 把 `sk-your-deepseek-key` 换成实际的 DeepSeek API key

---

## 第四步：新建其他 DeepSeek 项目（可选）

假如要在 `D:\新项目` 用 DeepSeek：

1. 创建文件夹 `D:\新项目`
2. 在 `D:\新项目\.claude\` 下新建 `settings.json`
3. 复制第三步的 env vars（改一下 API key 或保持一致）

---

## 使用方式

### 场景 1：原生 Claude（默认）

- 打开任何新项目（无 `.claude/settings.json`）
- 自动用全局配置 → 原生 Claude ✅

### 场景 2：切换到 DeepSeek

- 在 Claude Code 中打开 `D:\Deepseek_Agent` 文件夹
- 自动加载项目级 `settings.json` → DeepSeek ✅

### 场景 3：切换回原生

- 关闭 Claude Code，打开 `D:\Claudecode` 或任何其他项目
- 回到全局配置 → 原生 Claude ✅

> **无需 cc-switch，无需手动切换！**

---

## 常用命令

### 查看当前模型

```
/model
```

### 临时切换模型（仅当前会话）

```
/model opus
/model deepseek-v4-pro
```

### 查看当前项目配置

```
/config
```

---

## 故障排查

### git 在 PowerShell 里还是找不到

- 已修复 PATH，但需要重启 Claude Code
- 如果还不行，用 Git Bash Here 仍可正常工作

### office-word-MCP 不工作

- 检查 `C:\Users\{username}\.claude\.mcp.json` 是否存在
- 检查 Python 路径是否正确：`D:\Git\Office-Word-MCP-Server\word_mcp_server.py`
- 重启 Claude Code

### 切换项目后模型没变

- 确认新项目的 `.claude\settings.json` 是否包含 env vars
- 重启 Claude Code
- 运行 `/model` 查看当前模型

---

## 对比：cc-switch vs 项目级配置

| 特性 | cc-switch 方案 | 项目级配置方案（新） |
|------|---------------|-------------------|
| **工具依赖** | 需要 cc-switch.exe | 无外部工具 |
| **切换方式** | 手动运行工具 | 打开项目文件夹 |
| **多项目支持** | 🟢 OK | 🟢 OK（更简洁） |
| **自动化程度** | 手动 | 自动（项目级覆盖） |
| **配置位置** | 全局 + 工具 | 全局 + 项目级 |
| **维护复杂度** | 中等 | 低 |

---

## 版本历史

- **v1.0** (2026-06-09)：项目级配置方案初版，替代 cc-switch

---

## 附录：项目级 settings.json 模板

### 仅用原生 Claude

```json
{
  "model": "haiku"
}
```

### 用 DeepSeek

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-deepseek-key",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_MODEL": "deepseek-v4-pro"
  }
}
```

### 带权限配置

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-deepseek-key",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_MODEL": "deepseek-v4-pro"
  },
  "permissions": {
    "allow": [
      "PowerShell(*)",
      "Bash(*)"
    ]
  }
}
```
