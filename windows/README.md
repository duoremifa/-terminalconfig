# Windows Terminal 配置说明

本目录包含 Windows 上复现统一终端外观所需的全部文件。

## 前置条件

### 1. 安装 Sarasa Mono SC 字体（更纱黑体）

**无需管理员权限**，使用 per-user 安装：

```powershell
# 下载字体 (v1.0.39) - 使用 gh-proxy 镜像加速
$url = "https://gh-proxy.com/https://github.com/be5invis/Sarasa-Gothic/releases/download/v1.0.39/SarasaMonoSC-1.0.39.zip"
$zip = "$env:TEMP\sarasa.zip"
$extract = "$env:TEMP\sarasa"
Invoke-WebRequest -Uri $url -OutFile $zip -UseBasicParsing
Expand-Archive -Path $zip -DestinationPath $extract -Force
```

```powershell
# Per-user 安装到用户字体目录
$fontDir = "$env:LOCALAPPDATA\Microsoft\Windows\Fonts"
if (-not (Test-Path $fontDir)) { New-Item -ItemType Directory -Path $fontDir | Out-Null }

Get-ChildItem $extract -Filter "*.ttf" | ForEach-Object {
    Copy-Item $_.FullName $fontDir -Force
    # 注册到当前用户
    $regPath = "HKCU:\Software\Microsoft\Windows NT\CurrentVersion\Fonts"
    $fontName = $_.BaseName + " (TrueType)"
    Set-ItemProperty -Path $regPath -Name $fontName -Value "$fontDir\$($_.Name)"
}
```

```powershell
# 注销并重新登录一次，让 Font Cache 服务加载新字体
# （没有管理员权限无法重启 Font Cache 服务）
shutdown /l
```

重登后，字体即生效。

### 2. 安装 Windows Terminal

```powershell
winget install Microsoft.WindowsTerminal
```

或从 Microsoft Store 安装。

## 应用配置

### 方法 A：直接替换配置文件（推荐）

```powershell
$target = "$env:LOCALAPPDATA\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json"

# 先备份原配置
Copy-Item $target "$target.bak" -ErrorAction SilentlyContinue

# 复制本仓库的配置
Copy-Item "$PSScriptRoot\settings.json" $target -Force
```

重启 Windows Terminal 即可生效。

### 方法 B：在 GUI 中导入配色

1. 打开 Windows Terminal → Settings（`Ctrl+,`）
2. 左侧 **Color schemes** → **Add new** → **Import**
3. 把 `settings.json` 的 `schemes` 数组里 `macOS Dark` 那一段复制到一个单独 JSON 文件导入
4. 设置默认 scheme 为 `macOS Dark`
5. 设置默认字体为 `Sarasa Mono SC`，字号 10-13

## 配置亮点

这份 `settings.json` 包含以下设定：

### 外观
- **配色**: macOS Dark（自定义 16 色方案，与 macOS 版一致）
- **字体**: Sarasa Mono SC（更纱黑体），10pt，正常字重
- **光标**: 竖线（bar）
- **背景**: 85% 不透明度 + Acrylic 模糊
- **滚动条**: 隐藏
- **内边距**: 左右 12px，上下 16px

### 默认 Profile
- Windows PowerShell（无 Logo）
- 启动目录: `%USERPROFILE%`
- 也保留 Command Prompt profile

### 快捷键
- `Alt+Shift+-` : 水平分屏
- `Alt+Shift++` : 垂直分屏
- `Ctrl+Shift+W` : 关闭当前面板

## 卸载 / 回滚

```powershell
# 恢复备份
$target = "$env:LOCALAPPDATA\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json"
Copy-Item "$target.bak" $target -Force

# 删除字体
Remove-Item "$env:LOCALAPPDATA\Microsoft\Windows\Fonts\Sarasa*" -Force
# 并从 HKCU:\Software\Microsoft\Windows NT\CurrentVersion\Fonts 中删除对应条目
```

注销重登一次即可完全还原。
