---
id: "docs-fia-installation"
slug: "installation"
title: "安装与本地开发"
description: "Bun 1.4.4 与固定框架归档。"
date: "2026-10-31"
---

当前框架随 Fn 0.7 共仓交付，以下步骤从 Fn 源码构建并独立消费框架归档；独立 npm 发布继续暂停。

当前活跃源码位于 Fn 的 framework/fia。macOS 14+ Apple Silicon 为运行平台，框架维护需要 Swift 6。普通应用消费预编译 Host，无需自己的 Swift 工程。

在 Fn 根运行 `bun run framework:prepare`、`bun run framework:pack`、`bun run framework:verify`，使用已验收的归档安装 @semicoder/fia。生成应用后运行 `bun run dev`；`fia run` 使用隔离 Preview 运行打包代码，`fia build --dmg` 生成本地镜像。

固定运行时包含 Bun 1.4.4。开发和生产用同一 Bun HTML/React/CSS Modules 构建路径，修改后整页刷新。不要以公共包的版本号替代本地归档摘要。
