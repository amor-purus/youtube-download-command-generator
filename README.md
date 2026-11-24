# YouTube 下载命令生成器

面向 Windows 11 的中文 PowerShell 命令生成器，适用于需要日语字幕的授权视频、切片及直播回放。

## 使用方式

- 在线版：启用 GitHub Pages 后，访问 https://amor-purus.github.io/youtube-download-command-generator/
- 离线版：在仓库中下载 [index.html](index.html) 原始文件，双击打开即可，无需运行服务器。
- 输入 HTTPS YouTube 视频链接，选择画质、并发数、字幕与保存路径，复制命令到 PowerShell 运行。

## 功能

- 360p 至 8K 或最高可用画质（画质为上限）。
- 1–64 路分片并发；默认 16。
- 日语投稿者字幕、自动生成字幕，SRT 转换及可选内嵌。
- MP4 / MKV 容器，自定义绝对保存路径。
- 画质及字幕检测命令、复制命令、浏览器本地配置记忆。
- 不依赖外部前端库，不发送用户输入，不保存视频链接。

## 安装依赖

已安装并配置 PATH 的用户可跳过。在 PowerShell 执行：

```powershell
winget install --id yt-dlp.yt-dlp -e
winget install --id Gyan.FFmpeg -e
winget install --id DenoLand.Deno -e
```

重新打开 PowerShell 并运行：

```powershell
yt-dlp --version
ffmpeg -version
ffprobe -version
deno --version
```

## 部署 GitHub Pages

仓库 Settings → Pages → Build and deployment → Source: Deploy from a branch → main → / (root) → Save。私有仓库是否支持 Pages 取决于 GitHub 套餐；请自行决定是否公开仓库。

## 注意事项

- 本工具只生成命令，不在浏览器中下载视频。请仅下载获得授权或平台规则允许的内容。
- 高并发只适用于支持分片并发的协议，可能触发限流；不保证加速。
- 字幕必须由视频源提供；没有日语字幕时不会自动转录。
- MP4 封装取决于实际视频/音频编码兼容性；遇到兼容问题可选择 MKV。
- 依赖官方文档：[yt-dlp](https://github.com/yt-dlp/yt-dlp)、[FFmpeg](https://ffmpeg.org/)、[Deno](https://deno.com/)。

## 下载并发性能基准测试

网页新增独立基准测试区，复用上方视频链接、画质，也可输入 `yt-dlp -F` 列出的单一格式 ID。选择 180 秒以内、已结束且有授权的短视频。默认候选 1/4/8/16/32，每组 3 次，可加入 64；至少 2 个候选且必须包含 1。

1. 复制测试脚本到 Windows PowerShell 5.1 执行（或保存 UTF-8 BOM 的 .ps1）。
2. 首先解析实际选择的格式并锁定单一视频流；仅 `http_dash_segments` / `m3u8_native` 执行原生分片并发测试。普通 HTTPS 无需测试 -N。可选其他分片格式，但其结论不能用于普通 HTTPS 格式。
3. 估算总流量，输入 Y 后执行。每轮轮换顺序，每次重新下载完整单一媒体文件、不下载字幕、不合并音视频；记录真实媒体字节数与端到端耗时（含解析及连接），每次间隔 3 秒。预估体积不代表硬流量上限。
4. 失败、403/429 或实际协议无法确认时停止；每次删除测试媒体，仅保留本地日志。报告在 `$HOME\Downloads\YouTubeBenchmark` 的独立子目录。
5. `runs.csv` 记录每次耗时、速度、状态、重试信号；`summary.csv` 记录分组中位数；`report.html` 包含曲线、表格及推荐值。仅全部轮次成功且无重试信号的组进入比较，速度在最快组 95% 内时优先选低并发；未完成全部计划则不推荐。

基准测试不测服务器或软件的硬上限，不保证实际下载时仍然最优；建议在相同网络环境重复。原始日志可能包含临时媒体链接，请勿直接公开。未执行实际网络测试；PowerShell 运行仍需用户本机验收。
