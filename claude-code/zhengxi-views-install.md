# 📋 `zhengxi-views` Windows 安装最佳实践

> 项目地址：https://github.com/lyra81604/zhengxi-views
>
> 这是一个 **Agent Skill**（给 AI 助手用的技能包），专门做基金经理郑希（易方达）的观点溯源 + 投资方法应用。原生支持 Claude Code。

## 一、这是什么？

| 项 | 值 |
|---|---|
| 类型 | Agent Skill |
| 用途 | 基于郑希 2012–2026 全部公开观点原文，做可溯源问答 + 用他的方法给基金打分 |
| 技术栈 | Python 脚本 + Markdown 语料库 |
| 大小 | ~11 MB（`references/` 占 7MB，是语料和基金数据） |
| 原生支持 | Claude Code（最佳）/ 腾讯 WorkBuddy / Cursor / 其他 AI 也能用但功能受限 |

## 二、前置条件

| 项 | 要求 | 怎么装 |
|---|---|---|
| **Python** | 3.8+ | https://www.python.org/downloads/ （装时**勾上 "Add to PATH"**） |
| **Claude Code** | 最新版 | `npm install -g @anthropic-ai/claude-code`（需先装 Node.js） |
| **pip** | 随 Python 自带 | `python --version` 和 `pip --version` 验证 |
| **网络** | 下载时需能访问 GitHub；之后脚本抓数据走天天基金，国内直连 | — |

## 三、下载（三种方式）

### 方式 1：直接下载 ZIP（最简单，推荐）

浏览器打开 https://github.com/lyra81604/zhengxi-views → 绿色 **Code** 按钮 → **Download ZIP**

如果 GitHub 直连慢/打不开，用 gh-proxy 镜像 👇 直接粘到浏览器地址栏回车：

```
https://gh-proxy.com/https://github.com/lyra81604/zhengxi-views/archive/refs/heads/main.zip
```

下载后解压到任意位置，比如 `D:\zhengxi-views-main\`。

### 方式 2：git clone（想持续更新的话推荐）

```powershell
# 直连（如果网络可以）
git clone https://github.com/lyra81604/zhengxi-views.git

# 走 gh-proxy 镜像（如果直连慢）
git clone https://gh-proxy.com/https://github.com/lyra81604/zhengxi-views.git
```

### 方式 3：只下载必要文件（省空间）

`assets/` 是 demo 截图（~3.8MB），可以不要。只要这些：
- `SKILL.md`
- `README.md`
- `references/` （整个目录）
- `scripts/` （整个目录）
- `requirements.txt`

## 四、安装到 Claude Code（Windows）

### 步骤 1：创建 skills 目录并复制文件

打开 PowerShell：

```powershell
# 假设解压/clone 到 D:\zhengxi-views-main\，按实际路径改
$src = "D:\zhengxi-views-main"
$dst = "$HOME\.claude\skills\zhengxi-views"

New-Item -ItemType Directory -Force $dst | Out-Null
Copy-Item -Recurse "$src\SKILL.md", "$src\README.md", "$src\references", "$src\scripts" $dst
```

### 步骤 2：装 Python 依赖（仅"抓基金数据 / 评分"需要）

```powershell
pip install requests beautifulsoup4 lxml
```

> 💡 如果 `pip` 命令找不到，试 `python -m pip install ...`
>
> 💡 国内 pip 慢的话加清华源：
> ```powershell
> pip install -i https://pypi.tuna.tsinghua.edu.cn/simple requests beautifulsoup4 lxml
> ```

### 步骤 3（可选）：跳过 Claude Code 的 python 运行确认

默认 Claude Code 每次运行 python 都要你按确认。想省这一步，编辑 `%USERPROFILE%\.claude\settings.json`：

```json
{
  "permissions": {
    "allow": ["Bash(python:*)", "Bash(python3:*)"]
  }
}
```

> ⚠️ 这会放行**所有** python 命令，仅建议个人机器使用。随时可以删掉这行。

### 步骤 4：完全重启 Claude Code

**必须**完整退出再重开，不然 skill 不会加载：

```powershell
Get-Process claude -ErrorAction SilentlyContinue | Stop-Process -Force
# 然后重新打开终端
claude
```

## 五、验证安装成功

进入 Claude Code，直接问：

```
郑希怎么看光通信？
```

如果 skill 正确加载，回答里会**带原文引用**（如"他在 2026 年 6 月接受中国证券报采访时说..."）。

如果回答泛泛而谈、没有出处，说明 skill 没加载成功 —— 检查：
- 路径对吗？`%USERPROFILE%\.claude\skills\zhengxi-views\SKILL.md` 文件存在吗？
- 重启了吗？必须完整重启 Claude Code
- SKILL.md 内容对吗？里面应该有 YAML frontmatter（`---` 开头）

## 六、脚本单独使用（不用 AI 也能跑）

```powershell
cd "$HOME\.claude\skills\zhengxi-views"

# 在语料里搜"光通信"
python scripts\search_corpus.py "光通信"

# 查全市场某只基金代码
python scripts\fund_lookup.py "中欧医疗"

# 抓某只基金的真实数据
python scripts\fetch_any_fund.py 003095

# 用郑希框架给某只基金打分
python scripts\score_fund.py "招商中证白酒"
```

## 七、常见问题

### Q1：网络访问不了 GitHub 怎么办？

**方式 1 的 gh-proxy 镜像**（见第三节）能解决 99% 的情况。

如果连 gh-proxy 都慢，让他把下载好的 ZIP 通过微信 / 网盘发过去就行。

### Q2：装完问问题，Claude Code 报错 `python` 找不到？

Python 没装好或没加到 PATH。重新运行 Python 安装程序，**勾上 "Add Python to PATH"**，或者手动添加到系统环境变量 `Path`。

### Q3：`pip install` 报 SSL 错误？

公司网络可能拦截了 HTTPS。换源：

```powershell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn requests beautifulsoup4 lxml
```

### Q4：只想做"溯源问答"，不想装 Python？

可以。纯问答（查郑希观点、读他的方法、看他 8 只基金的数据）只需要 Claude Code 能读文件，**不需要 Python 依赖**。

但"抓全市场任意基金数据"和"用郑希框架打分"这两个功能必须装 Python。

### Q5：过段时间怎么更新 skill？

重新下载 ZIP 覆盖 `%USERPROFILE%\.claude\skills\zhengxi-views\` 里的文件，重启 Claude Code 即可。

## 八、快速对照清单

照这个顺序走：

- [ ] Python 3.8+ 已装，`python --version` 能跑
- [ ] Claude Code 已装，`claude --version` 能跑
- [ ] 下载了 `zhengxi-views` 仓库（ZIP 或 git clone）
- [ ] 复制到 `%USERPROFILE%\.claude\skills\zhengxi-views\`（含 `SKILL.md`、`references/`、`scripts/`）
- [ ] 跑 `pip install requests beautifulsoup4 lxml`
- [ ] （可选）在 `%USERPROFILE%\.claude\settings.json` 加 python 放行规则
- [ ] 完全重启 Claude Code
- [ ] 测试问"郑希怎么看光通信"，应有原文引用
