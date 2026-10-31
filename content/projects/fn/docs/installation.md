---
id: "docs-fn-installation"
slug: "installation"
title: "安装 Fn"
description: "下载 macOS 桌面平台并安装应用。"
date: "2026-11-01"
---

## macOS 安装

Fn 0.7.0（build 15）面向 macOS 14+、Apple Silicon。下载完整安装包，打开 DMG，将 **Fn.app** 拖入 Applications，再从「应用程序」启动。

[下载 Fn 0.7.0](https://github.com/semicoder-dev/fn/releases/download/v0.7.0/Fn-0.7.0-15-mac-arm64.dmg) · [发行说明与校验文件](https://github.com/semicoder-dev/fn/releases/tag/v0.7.0)

安装包内置 Bun，使用 Fn 无需自行安装开发工具。桌面正式版暂不提供手机连接。

## 安装应用

点击底部 Dock 的「应用」，从官方市场安装翻译、笔记和看板。也可添加自定义 HTTPS 市场或通过「本地安装」选择 `.fn.tgz`。

安装后从桌面卡片打开应用。笔记与看板可直接使用；翻译需在「设置 → 模型」登录 ChatGPT 并选择默认模型。

## 源码开发

需要 Bun 1.4.4、Xcode/Swift 6。取得源码后在 Fn 根目录运行：

```sh
bun install --frozen-lockfile --ignore-scripts
bun run dev
# 隔离预览打包代码
bun run run
# 构建本地测试安装包
bun run build --dmg
```

Dev、Preview 使用独立身份和数据。`bun dev` 与 `bun run dev` 等价；保持终端运行，按 Ctrl-C 退出。本机开发通信自动直连，无需关闭 HTTP 代理。标题栏右侧「在浏览器中打开」连接同一真实宿主。

继续阅读[第一次使用](/fn/docs/getting-started)、[应用管理](/fn/docs/plugins)与[应用开发](/fn/docs/plugin-development)。
