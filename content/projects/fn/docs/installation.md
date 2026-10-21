---
id: "docs-fn-installation"
slug: "installation"
title: "下载与安装"
description: "下载 fn 0.5.0，安装 Cordis 官方应用，了解独立数据目录与手机内测。"
date: "2026-10-21"
---

## 下载 fn

当前版本为 **fn 0.5.0 / build 10**，支持 **macOS 14+ / Apple Silicon**。应用和 DMG 使用 Developer ID 签名，DMG 已通过 Apple 公证并附带票据。

[下载 fn 0.5.0（Apple Silicon）](https://github.com/semicoder-dev/fn/releases/download/v0.5.0/fn-0.5.0-10-mac-arm64.dmg) · [发行说明](https://github.com/semicoder-dev/fn/releases/tag/v0.5.0)

打开 DMG，将 `fn.app` 拖入「应用程序」并启动。普通用户无需安装 Bun、Node.js 或 Swift。

安装包 SHA-256：

```text
c4dbd5fe51c30f8318476eef3ad1ee3c3301aaed5e3df7e6db96924f431e3a02
```

## 从旧版本升级

0.5 使用 Bun 宿主和原版 dsh / Cordis 内核，必须下载并替换完整 DMG。旧 SDK、插件、模型连接与 Chat v2 不兼容，需要重新配置连接、添加模型并安装新版应用。

新数据写入各模式数据目录的 `Backend/cordis-v1/`，不会读取、迁移或删除旧数据。不要将旧目录复制到新目录。手机须配合新的 Chat v3 内测客户端；运行目录回滚与数据库格式回滚分别处理，不能将新版数据直接交给旧内核。

## 安装官方应用

宿主不内置插件。在「应用中心」安装 **译文、随手记、网络搜索**，三者均为 Cordis **0.1.0**，精确消费公开 SDK **0.4.0**。随手记只需本地 KV；译文与搜索需要先在模型服务添加连接及可用模型。

[官方应用仓库](https://github.com/semicoder-dev/fn-plugins) 使用 GitHub Releases 分发完整归档。新市场为 `https://raw.githubusercontent.com/semicoder-dev/fn-plugins/main/marketplace-cordis.json`；旧市场保留旧契约，不供新版宿主消费。

插件安装和更新先生成并验证完整运行目录，任务空闲后重启后台，窗口保留并重连。插件属于可信本地代码，只安装可信来源。接着阅读[第一次使用](/fn/docs/getting-started)和[插件管理](/fn/docs/plugins)。

从官方仓库构建新应用，在 `examples/cordis/` 执行：

```bash
bun install --frozen-lockfile --ignore-scripts
bun run check
bun run test
bun run pack
bun catalog.ts check
```

## 插件开发与 SDK

SDK **0.4.0** 已公开，可以独立安装：

```bash
bun add --dev --exact @semicoder/fn-sdk@0.4.0
```

应用菜单可安装 `fn` CLI，使用 `fn application create/check/build/pack`；不保留旧 `plugin` 命令。后端 exports 指向 ESM TypeScript，由 Bun 执行；可选客户端入口必须预构建，并共享宿主的 React 和 UI。按[插件开发](/fn/docs/plugin-development)设置精确核心 peer 版本。

## 从源码构建

[fn v0.5.0 源码](https://github.com/semicoder-dev/fn/tree/v0.5.0) 需要 Bun 1.4.3 和 Swift 6 / Xcode 工具链。在仓库根执行：

```bash
bun install --frozen-lockfile --ignore-scripts
bun run check
bun run build
```

产物为 `dist/fn.app`，本机构建没有官方发行的签名公证证明。FIA 在同仓 `framework/fia/` 构建，框架报告记录实际源码和产物摘要；不依赖相邻 FIA 仓库。公开 FIA npm 0.18.0 与当前共仓源码不是同一产物，独立 npm 发布继续暂停。

日常体验用 `bun run dev`（fn Dev）或 `bun run run`（fn Preview）。它们分别使用 `.fia/dev/data/com.semicoder.fn`、`.fia/preview/data/com.semicoder.fn`，禁用正式更新源。直接打开 `dist/fn.app` 使用正式应用数据目录。

## 后续更新

应用更新源为 `https://semicoder-dev.github.io/fn/updates/latest.json`。启动时及每 24 小时检查，安装前需确认；后台任务忙碌时推迟，原生运行时改变需完整 DMG。插件市场与应用更新源独立管理。

## 手机内测

iPhone **0.3.0（4）** 已在既有内部 TestFlight 组分发，要求 iOS 17+；目前没有公开邀请。手机和桌面使用同一 iCloud 账号，Chat v3 支持多个 Session、已添加模型筛选、自定义交互卡片、审批、离线队列和 Markdown。桌面模型凭证不传到手机。

生产 CloudKit v3 schema 和两环境订阅已部署验证；新版本的真机卡片交互、跨设备模型选择和锁屏通知点击仍待实测，上传成功不代表这些设备场景已经通过。旧 Chat v2 历史保留，不迁移到新 zone。独立 Mac Chat 暂无公开安装包，需要从 `clients/apple/` 本地构建并配置匹配的签名。
