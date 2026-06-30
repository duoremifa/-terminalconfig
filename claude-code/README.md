# Claude Code Mac 安装配置

把 Windows 上的 Claude Code 使用体验搬到 macOS。

Move your Claude Code workflow from Windows to macOS — same editor experience, same domestic-model routing via CC-Switch.

---

## 总览 / Overview

整体架构：

```
┌────────────────┐       ┌────────────────┐      ┌──────────────────┐
│  Terminal.app  │  ──▶  │  Claude Code   │ ──▶  │   CC-Switch      │
│  或 iTerm2     │       │  (CLI 前端)     │      │  (本地代理)       │
└────────────────┘       └────────────────┘      └──────────────────┘
                                                          │
                                                          ▼
                                                  ┌──────────────────┐
                                                  │ 百炼 / Bailian   │
                                                  │ GLM / Qwen /     │
                                                  │ DeepSeek / Kimi  │
                                                  └──────────────────┘
```

- **Claude Code** 是 CLI 前端（Anthropic 官方出的 Node 工具）
- **CC-Switch** 在本地起一个代理，把 Claude Code 的 API 调用转到国内模型
- 你用的模型：GLM-5.2、通义千问 3.7 Plus、DeepSeek v4 Pro、Kimi K2.6（都走百炼）

## 步骤 1：安装 Node.js（v18+）

```bash
# 如果还没装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 装完后按提示加入 PATH（M 系列芯片 Mac 必做）
# 一般是在 ~/.zprofile 里加两行 eval，安装脚本会打印出来

# 装 Node
brew install node

# 验证
node --version   # 应该 v18+
npm --version
```

## 步骤 2：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code

# 验证
claude --version
```

> 如果遇到权限问题（npm 全局安装失败），用 Homebrew 方式或者装 nvm：
> ```bash
> curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
> source ~/.zshrc
> nvm install --lts
> npm install -g @anthropic-ai/claude-code
> ```

## 步骤 3：安装 CC-Switch（Mac 版）

下载地址：https://github.com/farion1231/cc-switch/releases

下载 **`CC-Switch-v{version}-macOS.dmg`**（已经 Apple 公证，可以直接打开）。

安装后首次启动：

1. 打开 CC-Switch
2. 在 Providers 里添加 **百炼 / Alibaba Cloud Bailian**
3. 填入你的百炼 API Key（https://bailian.console.aliyun.com/ 申请）
4. 在 Models 里配置模型映射（和 Windows 上一样）：
   - Opus slot → `bailian/qwen3.7-plus`
   - Sonnet slot → `bailian/kimi-k2.6`
   - Haiku slot → `bailian/deepseek-v4-pro`
   - Fable slot → `bailian/glm-5.2`
5. 启动代理（默认端口 `15721`，和 Windows 一致）

## 步骤 4：应用配置文件

把本目录下的 `settings.json` 复制到 `~/.claude/settings.json`：

```bash
mkdir -p ~/.claude
cp settings.json ~/.claude/settings.json
```

## 步骤 5：配置 shell 别名（可选）

```bash
cat >> ~/.zshrc << 'EOF'

# Claude Code 快捷启动
alias ai="claude"

# 如果用 CC-Switch 默认端口之外的端口，需要改这个
# export ANTHROPIC_BASE_URL="http://127.0.0.1:15721"
EOF

source ~/.zshrc
```

## 步骤 6：启动

```bash
# 先确保 CC-Switch 代理已经启动（看菜单栏图标）

# 然后
claude
# 或者用别名
ai
```

第一次启动会走一遍 onboarding（接受条款等），之后就直接进入交互界面。

---

## 配置说明

### settings.json 关键字段

| 字段 | 作用 |
|---|---|
| `env.ANTHROPIC_AUTH_TOKEN = "PROXY_MANAGED"` | 告诉 Claude Code 不要自己处理认证，由 CC-Switch 代理接管 |
| `env.ANTHROPIC_BASE_URL = "http://127.0.0.1:15721"` | 把请求发到本地 CC-Switch 代理 |
| `env.ANTHROPIC_DEFAULT_*_MODEL` | 四个档位（opus/sonnet/haiku/fable）对应的模型名 |
| `env.AUTO_COMPACT_ENABLED = "1"` | 自动压缩上下文（长对话时自动触发） |
| `env.CLAUDE_AUTOCOMPACT_PCT_OVERRIDE = "75"` | 上下文用到 75% 时自动压缩 |
| `theme = "dark"` | 暗色主题（和 Terminal.app 配色一致） |

### 如果你不用 CC-Switch

把 settings.json 里的 `env` 部分全部删掉，改成：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-..."
  },
  "theme": "dark"
}
```

然后启动时会直接走 Anthropic 官方 API（需要海外网络和付费账号）。

---

## 排错 / Troubleshooting

### `claude` 命令找不到

```bash
# 看 npm 全局安装路径在哪
npm config get prefix
# 把这个路径下的 bin 加到 PATH，比如：
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 启动后报 `401 Unauthorized` 或 `connection refused`

CC-Switch 没启动，或者端口不对：

```bash
# 检查代理是否在监听
lsof -i :15721
```

如果没输出，说明 CC-Switch 没启动或端口改了。去 CC-Switch 设置里看实际端口，把 settings.json 里的 `ANTHROPIC_BASE_URL` 改成对应端口。

### 中文乱码

Mac 默认是 UTF-8，一般没问题。如果看到乱码，检查：

```bash
echo $LANG
# 应该输出类似 en_US.UTF-8 或 zh_CN.UTF-8
```

如果不是，加到 `~/.zshrc`：

```bash
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
```

### 模型响应慢 / 超时

国内模型通过百炼 API，网络一般没问题。但百炼有 QPS 和 TPM 限制，长对话可能会触发限速。可以：
- 降低 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`（更早压缩）
- 在 CC-Switch 里切到更快的模型（比如把 Opus 档换成 GLM-5.2）

---

## 文件清单 / Files

- `settings.json` — Claude Code 用户配置（对应 `~/.claude/settings.json`）
- `zshrc.append` — 可选的 shell 配置片段（追加到 `~/.zshrc`）
- `CLAUDE.md` — 项目级指令模板（放到任意项目根目录，Claude Code 会自动读取）
- `README.md` — 本文档
