# Terminal Config / 终端配置

A consistent terminal look across Windows and macOS — same font (**Sarasa Mono SC / 更纱黑体**), same color scheme (**macOS Dark**).

在 Windows 和 macOS 上保持一致的终端外观：同一字体（**更纱黑体**），同一配色（**macOS Dark**）。

---

## Repository layout / 目录结构

```
.
├── macos/
│   ├── macOS-Dark.itermcolors   # iTerm2 颜色预设 / iTerm2 color preset
│   ├── terminal-app-setup.md    # 系统自带 Terminal.app 配置说明
│   └── README.md                # macOS 完整安装步骤
├── windows/
│   ├── settings.json            # Windows Terminal 配置（原样备份）
│   └── README.md                # Windows 完整安装步骤
└── README.md                    # 本文件 / this file
```

## Quick start / 快速开始

### macOS

1. 安装 Sarasa Mono SC 字体：`brew install --cask font-sarasa-mono-sc`
2. 安装 iTerm2（如未安装）：`brew install --cask iterm2`
3. 双击 `macos/macOS-Dark.itermcolors` 导入配色
4. 在 iTerm2 Profile → Colors → Color Presets 选 `macOS Dark`，字体设为 `Sarasa Mono SC` 13pt

详细步骤见 [`macos/README.md`](macos/README.md)。

### Windows

1. 安装 Sarasa Mono SC 字体（per-user，无需管理员）
2. 安装 Windows Terminal：`winget install Microsoft.WindowsTerminal`
3. 把 `windows/settings.json` 复制到 `%LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json`

详细步骤见 [`windows/README.md`](windows/README.md)。

## Color scheme: macOS Dark

| Role           | Hex       |
|----------------|-----------|
| Background     | `#1E1E1E` |
| Foreground     | `#DDDDDD` |
| Cursor         | `#FFFFFF` |
| Selection      | `#264F78` |
| Black / Bright | `#1E1E1E` / `#7F7F7F` |
| Red / Bright   | `#E5222B` / `#FF6E62` |
| Green / Bright | `#19C718` / `#5FF96B` |
| Yellow / Bright| `#E5A922` / `#FFC95E` |
| Blue / Bright  | `#2B66FF` / `#6C9EFF` |
| Purple / Bright| `#C027B4` / `#E667D9` |
| Cyan / Bright  | `#19A9C7` / `#5CE3F0` |
| White / Bright | `#DDDDDD` / `#FFFFFF` |

## License

配置本身无版权限制，随意使用。字体 Sarasa Mono SC 遵循 SIL Open Font License 1.1。
