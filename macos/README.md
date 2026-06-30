# macOS Terminal Setup / macOS 终端配置

本目录包含在 macOS 上复现与 Windows 一致的终端外观所需的全部文件。

This directory contains everything needed to reproduce the Windows terminal look on macOS.

---

## Prerequisites / 前置条件

### 1. 安装 Sarasa Mono SC 字体（更纱黑体）

```bash
# 如果还没装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装字体
brew install --cask font-sarasa-mono-sc
```

装好后在「字体册 / Font Book」里能看到 `Sarasa Mono SC` 即可。

If Homebrew is not installed:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install --cask font-sarasa-mono-sc
```

### 2. 选择终端应用

macOS 有两个主流选项：

| 应用 | 特点 | 推荐 |
|---|---|---|
| **iTerm2** | 第三方，功能丰富，配色导入最简单 | ⭐ 首选 |
| **Terminal.app** | 系统自带，无需安装 | 轻量需求 |

下面分别说明。

---

## Option A — iTerm2（推荐）

### 安装 iTerm2

```bash
brew install --cask iterm2
```

或从 https://iterm2.com/downloads.html 下载。

### 导入配色

1. 双击本目录下的 `macOS-Dark.itermcolors`
2. iTerm2 会弹出确认导入窗口，点 **Import**
3. 打开 iTerm2 → Settings（`⌘,`）→ Profiles → Colors
4. 右下角 **Color Presets…** → 选 `macOS Dark`

### 字体与外观

Settings → Profiles → Text：

- **Font**: `Sarasa Mono SC`
- **Size**: `13`
- **Cursor**: 选 `Vertical Bar`（竖线光标）
- 勾选 `Blinking cursor`（如需要）

Settings → Profiles → Window：

- **Transparency**: ~15%
- 勾选 **Blur**，值 ~10%

### 快捷键（可选，对齐 Windows Terminal）

Settings → Keys → Key Bindings → 点 `+` 逐条添加：

| 按键 | Action | Parameters |
|---|---|---|
| `⌥⇧-` | Split Pane Vertically | — |
| `⌥⇧=` | Split Pane Horizontally | — |
| `⇧⌘W` | Close Pane | — |
| `⇧⌘T` | New Tab | — |
| `⇧⌘[` | Select Previous Tab | — |
| `⇧⌘]` | Select Next Tab | — |

---

## Option B — Terminal.app（系统自带）

Terminal.app 的配置文件（`.terminal`）中颜色是 NSArchiver 二进制格式，不适合文本分发，因此用 GUI 手动配置。约 5 分钟完成。

### 步骤

1. 打开 Terminal.app → 菜单栏 `Terminal` → `Settings...`（或 `⌘,`）
2. 切到 **Profiles** 标签 → 右下角点 `+` 新建一个
3. 建议基于 **Pro** 模板开始（右键 Pro → Duplicate）

**Profile 标签：**

- **Name**: `macOS Dark`
- **Font**: 点 `Change...` → 选 `Sarasa Mono SC`，Regular，13pt
- **Cursor**: 选 `Vertical bar`（`|`）
- 勾选 **Use cursor as text insertion point**
- **Background opacity**: ~85%
- 勾选 **Blur**，值 ~20%

**Colors 标签：**

逐条点击颜色块，选 `Other...`，用 RGB 输入（Hex 代码 macOS 颜色面板支持直接粘贴）：

| 角色 | Hex |
|---|---|
| Text (前景) | `#DDDDDD` |
| Background | `#1E1E1E` |
| Bold Text | `#FFFFFF` |
| Cursor | `#FFFFFF` |
| Selection | `#264F78` |

**ANSI 颜色（点每个色块修改）：**

| 位置 | 颜色 | Hex |
|---|---|---|
| 黑 | Black | `#1E1E1E` |
| 红 | Red | `#E5222B` |
| 绿 | Green | `#19C718` |
| 黄 | Yellow | `#E5A922` |
| 蓝 | Blue | `#2B66FF` |
| 紫 | Magenta | `#C027B4` |
| 青 | Cyan | `#19A9C7` |
| 白 | White | `#DDDDDD` |
| 亮黑 | Bright Black | `#7F7F7F` |
| 亮红 | Bright Red | `#FF6E62` |
| 亮绿 | Bright Green | `#5FF96B` |
| 亮黄 | Bright Yellow | `#FFC95E` |
| 亮蓝 | Bright Blue | `#6C9EFF` |
| 亮紫 | Bright Magenta | `#E667D9` |
| 亮青 | Bright Cyan | `#5CE3F0` |
| 亮白 | Bright White | `#FFFFFF` |

### 设为默认

在 Profiles 列表里，右键新 profile → **Use as Default**。以后新开的窗口都会用这套配置。

---

## 验证 / Verify

打开新终端窗口，运行：

```bash
echo $TERM
# 应该显示 xterm-256-color

# 测试 256 色输出
for i in {30..37}; do echo -e "\e[${i}mColor ${i}\e[0m"; done
```

字体渲染确认：打开 `top` 或 `htop`，看 CJK 字符和拉丁字符是否基线对齐、字重一致。

---

## 文件清单 / Files

- `macOS-Dark.itermcolors` — iTerm2 颜色预设（双击导入）
- `terminal-app-setup.md` — Terminal.app 手动配置说明（即本文档的简化版，便于离线查看）
- `README.md` — 本文档
