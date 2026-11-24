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
