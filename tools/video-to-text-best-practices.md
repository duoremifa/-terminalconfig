# 🎬 视频转文字 / 翻译工具最佳实践

对比两个本地音视频转文字工具，并给出 Windows 安装步骤与使用建议。

- **Buzz** — 开源免费，基于 OpenAI Whisper，功能纯粹（转写 + 翻译）
- **Memo AI** — 商业软件（有免费版），功能更全（转写 + 翻译 + 摘要 + 思维导图 + 语音合成）

---

## 一、快速对比

| 维度 | Buzz | Memo AI |
|---|---|---|
| **开源** | ✅ MIT 开源 | ❌ 闭源 |
| **价格** | 完全免费 | 免费版 + 付费版（$25.99/年 或 $49.99 终身）|
| **底层引擎** | OpenAI Whisper（本地）| Whisper + 其他模型（本地）|
| **翻译** | ✅ Whisper 自带翻译（译成英文）| ✅ 谷歌/微软/AI 翻译，支持多语种互译 |
| **字幕格式** | SRT / VTT / TXT | SRT / VTT / TXT 等多种 |
| **GPU 加速** | ✅ 支持（含核显）| ✅ 付费版解锁；免费版仅 CPU |
| **批量处理** | ✅ | 付费版；免费版一次限 2 个文件 |
| **AI 摘要 / 思维导图** | ❌ | ✅ 付费版 |
| **语音合成 (TTS)** | ❌ | ✅ |
| **YouTube/播客链接直转** | ❌ 需先下载 | ✅ 直接粘链接 |
| **平台** | Win / Mac / Linux | Win / Mac |
| **数据隐私** | 完全离线 | 完全离线（数据不出设备）|
| **GitHub stars** | 19.9k ⭐ | — |
| **最新版本** | v1.4.4（2026-03）| 持续更新 |

---

## 二、怎么选？

### 选 Buzz 如果你 👇
- 想**完全免费**，不想花一分钱
- 只要**转写 + 字幕**，不需要摘要、思维导图这些花活
- 视频是**中文/英文**为主（Whisper 翻译只能译成英文，中英互译不如 Memo）
- 愿意自己先下载好视频文件再转
- 喜欢**开源**、可审计的软件

### 选 Memo AI 如果你 👇
- 需要**中英字幕互译**（Whisper 只能译成英文，Memo 能中→英、英→中）
- 想要**AI 摘要、思维导图**（看完视频要总结）
- 想直接粘 **YouTube / 播客链接**就转，不想先下载
- 愿意付 **$49.99 终身**（一次性，性价比高）或先用免费版试
- 要**批量处理**很多文件

### 我的推荐

**先装 Memo AI 免费版** —— 功能更全，免费版已经能转写 + 翻译，够用就停。

**如果免费版不够（要 GPU 加速 / 批量 / 摘要）**：
- 重度使用 → 花 $49.99 买终身版（Believer），一次买断
- 偶尔用 → 试试 Buzz（免费 + GPU 加速 + 批量，但翻译只能译成英文）

**两者可以共存**，不冲突。Memo 日常用，Buzz 当备用。

---

## 三、Buzz 安装指南（Windows）

### 下载

打开 https://github.com/chidiwilliams/buzz/releases/latest

下载这几个文件（同一版本，分片 + 启动器）：
- `Buzz-1.4.4-windows.exe`（4.3 MB，安装启动器）
- `Buzz-1.4.4-windows-1.bin`（约 2 GB，数据分片 1）
- `Buzz-1.4.4-windows-2.bin`（约 568 MB，数据分片 2）

> ⚠️ **三个文件必须放在同一个文件夹**，缺一不可（exe 是引导，两个 .bin 是真正的程序数据）。

如果 GitHub 直连慢，用 gh-proxy 镜像，把下面链接里的 URL 替换：
```
https://gh-proxy.com/https://github.com/chidiwilliams/buzz/releases/download/v1.4.4/Buzz-1.4.4-windows.exe
https://gh-proxy.com/https://github.com/chidiwilliams/buzz/releases/download/v1.4.4/Buzz-1.4.4-windows-1.bin
https://gh-proxy.com/https://github.com/chidiwilliams/buzz/releases/download/v1.4.4/Buzz-1.4.4-windows-2.bin
```

### 安装

1. 三个文件放同一文件夹
2. 双击 `Buzz-1.4.4-windows.exe` 运行安装
3. Windows SmartScreen 拦截的话 → "更多信息" → "仍要运行"
4. 安装完启动 Buzz

### 首次使用

1. 打开 Buzz → 导入一个音视频文件
2. 选模型（关键，见下面"模型选择"）
3. 第一次用某模型会自动下载（几百 MB ~ 几 GB，看模型大小）
4. 等转写完成，导出 SRT / TXT

### 模型选择（重要）

| 模型 | 大小 | 速度 | 准确率 | 显存/内存需求 | 适合 |
|---|---|---|---|---|---|
| `tiny` | 75 MB | 极快 | 一般 | 1 GB | 快速预览、纯英文清晰录音 |
| `base` | 145 MB | 很快 | 还行 | 1 GB | 日常中文短音频 |
| `small` | 480 MB | 快 | 不错 | 2 GB | **推荐起步**，中文效果好 |
| `medium` | 1.5 GB | 慢 | 好 | 5 GB | 高质量需求 |
| `large-v3` | 3 GB | 很慢 | 最好 | 10 GB | 最高质量，长视频 |
| `faster-whisper` | 同上 | **更快** | 同等 | 需 6GB+ 显存 GPU | 有独立显卡选这个 |

**建议**：
- 没独立显卡 → 从 `small` 开始，不够再升 `medium`
- 有 N 卡（6GB+ 显存）→ 直接 `faster-whisper large-v3`，速度飞快
- 中文视频 → 至少 `small`，`medium` 效果明显更好

### GPU 加速配置

Buzz 默认可能用 CPU。要开 GPU：
1. 安装时勾选 CUDA 支持（如果有 N 卡）
2. 或在设置里选 `faster-whisper` 模型（自动走 GPU）
3. 核显也能加速，在设置里看有没有 `OpenVINO` 选项

---

## 四、Memo AI 安装指南（Windows）

### 下载

打开 https://memo.ac/zh/ → 点 **下载**

或直接访问下载页（一般在官网首页有按钮）。

### 安装

1. 下载安装包（.exe）
2. 双击安装
3. SmartScreen 拦截 → "更多信息" → "仍要运行"
4. 启动后注册 / 登录账号（激活用）

### 免费版 vs 付费版

| 功能 | Basic（免费） | Pro / Believer（付费）|
|---|---|---|
| 转写 | ✅ 无限 | ✅ 无限 |
| 字幕翻译 | ✅ 无限 | ✅ 无限 |
| 语音合成 | ✅ 无限 | ✅ 无限 |
| GPU 加速 | ❌ 仅 CPU | ✅ |
| 高质量模型 | ❌ | ✅ |
| 智能断句 | ❌ | ✅ |
| 批量处理 | 限 2 个文件 | ✅ 无限 |
| AI 摘要 | ❌ | ✅ |
| 导出格式 | 基础 | 多种 |

**价格**：
- Pro：$25.99/年（内测价，原价 $39.99）
- Believer：**$49.99 终身**（内测价，原价 $99）← 推荐这个，一次买断

### 使用流程

1. 打开 Memo AI
2. 三种导入方式：
   - 拖入本地音视频文件
   - 粘贴 YouTube / 播客链接
   - 录音
3. 选语言（自动识别也行）
4. 选翻译目标语言（可选，比如英→中）
5. 开始转写
6. 完成后可：导出字幕 / 生成摘要 / 做思维导图 / TTS 朗读

### GPU 加速（付费版）

设置 → GPU 加速 → 选 CUDA（N 卡）/ OpenVINO（核显）

效果：**2 分钟转完 1 小时音频**（官方数据，实际看显卡）。

---

## 五、最佳实践技巧

### 1. 模型选择黄金法则
- **中文音频**：Whisper `small` 起步，要准确上 `medium`
- **英文音频**：`base` 就够，`small` 更准
- **多语言混杂**：用 `large-v3`，小模型会搞混
- **追求速度**：有 GPU 用 `faster-whisper`，没 GPU 接受 `tiny`/`base`

### 2. 提高准确率
- 音频质量越好，转写越准（背景噪音是大敌）
- 长视频分段处理（>1 小时容易出错或崩）
- Buzz 里可以调 `temperature` 参数（0 最保守，1 最有创造力，转写用 0）
- 转完手动校对专有名词、人名

### 3. 翻译注意
- **Buzz 的翻译**：Whisper 原生翻译**只能译成英文**（`--task translate`）。要中英互译得转完用别的翻译工具
- **Memo 的翻译**：支持中→英、英→中、任意语种互译，更实用
- 字幕翻译建议：先转写生成原文 SRT，再整体翻译，比边听边译准

### 4. 字幕格式
- 给视频配字幕 → **SRT**（通用）
- 网页 / 特定播放器 → **VTT**
- 只看文字 → **TXT**
- 要带时间戳校对 → SRT 用文本编辑器打开

### 5. 硬件建议
| 配置 | 推荐方案 |
|---|---|
| 无独显，8GB 内存 | Buzz + `small` 模型（CPU 慢但能用）|
| 无独显，16GB 内存 | Buzz + `medium`，或 Memo 免费版 |
| N 卡 6GB+ 显存 | Buzz + `faster-whisper large-v3`（最佳性价比）|
| N 卡 8GB+ 显存 | Memo 付费版 + GPU（最省心，功能全）|
| Mac M1/M2/M3 | 两者都行，Buzz ARM64 版 + Metal 加速 |

### 6. 隐私与版权
- 两个工具都**本地运行**，数据不上传 —— 适合公司内部会议、敏感内容
- 转写别人视频注意版权，个人学习研究没问题，公开发布需授权

---

## 六、常见问题

### Q1：Buzz 下载的 .bin 文件是什么？必须下吗？
必须下。.exe 只是 4MB 的启动器，真正的程序在两个 .bin 里（加起来约 2.5GB）。三个文件放同一目录才能安装。

### Q2：Buzz 模型下载很慢 / 失败？
模型从 Hugging Face 下载，国内可能慢。可以：
- 挂代理（GLaDOS 之类的）
- 或手动下载模型放到 Buzz 的模型目录（`%USERPROFILE%\.cache\whisper\`）

### Q3：Memo 免费版够用吗？
转写 + 翻译够用，但只能 CPU 跑（慢）、不能批量、没摘要。轻度使用够了，重度建议买终身版。

### Q4：转中文哪个准？
- Buzz：`medium` 或 `large-v3` 模型
- Memo：付费版高质量模型
- 两者用同模型准确率接近（都是 Whisper 系）

### Q5：能转 YouTube 视频吗？
- **Memo**：直接粘链接，一键转 ✅
- **Buzz**：要先下载视频（用 yt-dlp 之类），再导入文件

### Q6：长视频（>2 小时）怎么办？
- 分段转（用 ffmpeg 切分）
- 用 GPU 加速
- 关掉其他占内存的程序
- Buzz `large` 模型转 2 小时视频，CPU 可能要 6+ 小时，GPU 可能 20 分钟

### Q7：公司电脑没管理员权限能装吗？
- Buzz：安装需要管理员，但可以装到用户目录试试便携版（如果有）
- Memo：一般也需要管理员装，但装完后是用户级运行

---

## 七、总结推荐

| 场景 | 推荐 |
|---|---|
| 完全免费、开源控 | **Buzz** + `small` 模型 |
| 有 N 卡、要速度 | **Buzz** + `faster-whisper` |
| 要中英互译字幕 | **Memo AI**（免费版就够试）|
| 要摘要 / 思维导图 | **Memo AI** 付费版 |
| 直接转 YouTube 链接 | **Memo AI** |
| 重度长期使用 | **Memo AI Believer**（$49.99 终身）|
| 敏感内容、要隐私 | 两个都行（都本地运行）|

---

## 参考链接

- Buzz GitHub：https://github.com/chidiwilliams/buzz
- Buzz 官网：https://chidiwilliams.github.io/buzz
- Buzz 中文 FAQ：https://chidiwilliams.github.io/buzz/zh/docs/faq
- Buzz CSDN 教程：https://blog.csdn.net/qiuweifan/article/details/140812295
- Buzz 博客园教程：https://www.cnblogs.com/jzssuanfa/p/19612756
- 少数派 Whisper 指南：https://sspai.com/post/83644
- Memo AI 官网：https://memo.ac/zh/
- Memo AI 价格：https://memo.ac/zh/pricing
- Memo AI 使用指南：https://memo.ac/zh/guide/start-here
