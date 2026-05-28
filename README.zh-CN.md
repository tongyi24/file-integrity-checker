[English](README.md) | **中文**

# File Integrity Checker

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/demo-live-brightgreen)](https://tongyi24.github.io/file-integrity-checker/)
[![No Dependencies](https://img.shields.io/badge/dependencies-zero-orange)]()
[![Single File](https://img.shields.io/badge/size-single%20HTML-purple)]()

**[立即体验](https://tongyi24.github.io/file-integrity-checker/)** — 完全在浏览器中运行，无需下载。

一款便携、基于浏览器的文件完整性校验工具。零依赖、零安装、零网络请求。打开一个 HTML 文件，就能验证你的文件是否完好无损。

![Screenshot](docs/screenshot.png)

## 为什么需要这个工具

在不同设备之间传输大文件（Mac → Windows、NAS → 笔记本、云端 → 本地）时，文件损坏或不完整的情况并不少见——尤其是 4K 视频、RAW 照片或数据库备份。这个工具让你能逐字节确认文件是否完整到达，无需安装任何软件，也无需信任第三方服务器。

## 功能特性

| 功能 | 说明 |
|------|------|
| **文件夹对比** | 使用 SHA-256 并排对比两个文件夹，检测相同、已修改、缺失、多余和已移动的文件 |
| **文件对比** | 逐字节对比两个单独的文件 |
| **生成校验和** | 为任意文件夹生成与 `sha256sum` 兼容的校验和文件 |
| **校验校验和** | 根据之前生成的校验和文件验证文件 |
| **灵活匹配** | 跨设备校验，忽略目录结构差异——按文件名或内容哈希匹配 |
| **智能缓存** | 将哈希值缓存在 `localStorage` 中；未改动的文件在后续运行时瞬间完成校验 |
| **流式哈希** | 纯 JavaScript 实现的 SHA-256，以 4MB 分块处理文件——可处理数 GB 大文件，不会出现内存问题 |

## 快速开始

### 方式一：在线使用（推荐）

访问 **https://tongyi24.github.io/file-integrity-checker/** — 就这么简单。

### 方式二：本地使用

1. 下载 `index.html`（只有一个文件，约 25KB）
2. 用任意现代浏览器打开
3. 完成——完全离线可用

## 使用场景

- **视频 / 照片传输** — 验证通过 LocalSend、AirDrop、SMB 或 U 盘传输的 4K 素材
- **备份验证** — 确认 NAS/云端备份与原始文件一致
- **跨平台同步** — 在 Mac ↔ Windows ↔ Linux 之间传输后校验文件
- **数据迁移** — 确保数据库导出、压缩包或磁盘镜像传输正确

## 跨设备工作流

在不同设备之间传输文件后校验（例如 Mac → Windows）：

1. **源设备**：打开「生成校验和」标签页 → 选择文件夹 → 保存 `.txt` 校验和文件
2. **传输**：将校验和文件与数据一起复制（文件很小，只是文本）
3. **目标设备**：打开「校验校验和」标签页 → 加载校验和文件 → 选择目标文件夹
4. **目录结构不同时**：启用 **灵活匹配**，按文件名或内容哈希匹配，而非精确路径

## 工作原理

```
                    ┌─────────────┐
  选择文件 ────────▶ │  浏览器     │ ──▶ 结果
  (数据不离开      │  (本地 JS)  │     (通过/失败)
   你的设备)        └─────────────┘
                         │
                    纯 SHA-256
                    (4MB 分块)
```

- 通过浏览器的 File API 在本地读取文件
- SHA-256 哈希在纯 JavaScript 中运行（无 WebAssembly，不依赖 Web Crypto API）
- 结果只留在浏览器标签页中——不会发送到任何地方
- 缓存使用 `localStorage`，作用域限定在页面来源

## 隐私与安全

- **100% 客户端运行** — 所有文件处理都在你的浏览器中完成，不会上传到任何服务器。
- **零网络请求** — 没有 `fetch`、没有 `XMLHttpRequest`、没有追踪像素、没有分析统计。
- **无第三方代码** — 没有 CDN 引用、没有外部脚本、没有从 Google 加载字体。
- **缓存隔离** — `localStorage` 中的哈希缓存只存哈希字符串，从不存储文件内容。
- **开源** — 所有代码都在单个文件中，可随时审计。

## 技术细节

- **单 HTML 文件** — CSS 和 JavaScript 内嵌，无需构建步骤
- **约 25KB 总大小** — 比大多数 favicon 还小
- **SHA-256** — 手写的流式实现（消息调度、压缩、正确的填充）
- **4MB 分块读取** — 可处理数 GB 文件，不会造成内存压力
- **三级灵活匹配** — 路径 → 文件名 → 内容哈希（逐级回退）
- **localStorage 缓存** — 键格式为 `folderName/relativePath|fileSize|lastModified`

## 兼容性

在任何装有现代浏览器的操作系统上均可使用：

| 操作系统 | 浏览器 |
|----------|--------|
| macOS | Chrome、Safari、Edge、Firefox |
| Windows | Chrome、Edge、Firefox |
| Linux | Chrome、Firefox |

> 注意：部分版本的 Safari 对 `webkitdirectory` 支持有限。推荐使用 Chrome/Edge 以获得最佳体验。

## 与其他方案对比

| | 本工具 | 命令行 `shasum` | HashMyFiles | 在线哈希工具 |
|--|--------|----------------|-------------|--------------|
| 跨平台 | 全平台 | 各系统命令不同 | 仅 Windows | 取决于服务 |
| 安装 | 无需 | 系统自带工具 | 需下载安装 | 无需 |
| 隐私 | 100% 本地 | 本地 | 本地 | 文件上传到服务器 |
| 文件夹对比 | 内置 | 需手动脚本 | 不支持 | 不支持 |
| 灵活匹配 | 支持 | 不支持 | 不支持 | 不支持 |
| 缓存 | 支持 | 不支持 | 不支持 | 不支持 |
| 图形界面 | 有 | 无 | 有 | 有 |

## 贡献

欢迎贡献！由于这是单文件项目，请将所有改动保留在 `index.html` 内。

## 许可证

[MIT](LICENSE)
