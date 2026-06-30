# Terminal.app 手动配置说明

本文档是 [`README.md`](README.md) 中 Option B 的精简版，便于离线查看或打印。

## 1. 新建 Profile

1. 打开 Terminal.app → `Terminal` → `Settings...`（或 `⌘,`）
2. Profiles → 右下角 `+` 新建
3. 建议基于 **Pro** 模板：右键 Pro → Duplicate

设置：
- **Name**: `macOS Dark`
- **Font**: `Sarasa Mono SC`，Regular，13pt
- **Cursor**: `Vertical bar` (`|`)
- **Background opacity**: ~85%
- 勾选 **Blur**，值 ~20%

## 2. 颜色配置

在 Colors 标签，逐个颜色块点 `Other...`，用 RGB 输入（Hex 代码 macOS 颜色面板支持直接粘贴）：

### 基本色

| 角色 | Hex |
|---|---|
| Text (前景) | `#DDDDDD` |
| Background | `#1E1E1E` |
| Bold Text | `#FFFFFF` |
| Cursor | `#FFFFFF` |
| Selection | `#264F78` |

### ANSI 颜色

| 位置 | Hex |
|---|---|
| Black | `#1E1E1E` |
| Red | `#E5222B` |
| Green | `#19C718` |
| Yellow | `#E5A922` |
| Blue | `#2B66FF` |
| Magenta | `#C027B4` |
| Cyan | `#19A9C7` |
| White | `#DDDDDD` |
| Bright Black | `#7F7F7F` |
| Bright Red | `#FF6E62` |
| Bright Green | `#5FF96B` |
| Bright Yellow | `#FFC95E` |
| Bright Blue | `#6C9EFF` |
| Bright Magenta | `#E667D9` |
| Bright Cyan | `#5CE3F0` |
| Bright White | `#FFFFFF` |

## 3. 设为默认

在 Profiles 列表里，右键 `macOS Dark` → **Use as Default**。
