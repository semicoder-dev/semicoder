---
id: "docs-fn-installation"
slug: "installation"
title: "下载与安装"
description: "下载 fn 0.6.0，安装看板和模型目录，了解全插件化与不兼容升级边界。"
date: "2026-10-31"
---

## 下载 fn

当前版本为 **fn 0.6.0 / build 13**，支持 **macOS 14+ / Apple Silicon**。应用和 DMG 使用 Developer ID 签名，DMG 已通过 Apple 公证并附带票据。

[下载 fn 0.6.0（Apple Silicon）](https://github.com/semicoder-dev/fn/releases/download/v0.6.0/fn-0.6.0-13-mac-arm64.dmg) · [发行说明](https://github.com/semicoder-dev/fn/releases/tag/v0.6.0)

打开 DMG，将 `fn.app` 拖入「应用程序」并启动。普通用户无需安装 Bun、Node.js 或 Swift。启动进入应用中心，按需安装独立插件。

安装包 SHA-256：

```text
05dd07c0bd7534e2865fb974b9d34444503a4dde1216e7f516c64fa4ac7de8e8
```

## 从 0.5.x 或更早版本升级

0.6 进入全插件化阶段，使用新的 SDK、插件与数据契约。**必须退出 fn 后用完整 DMG 替换应用，并重新配置模型连接和安装新版插件。** 不能沿用 SDK 0.4 或更早插件。

新数据写入各模式数据目录的 `Backend/plugins-v1/`，不读取、迁移或删除旧 `cordis-v1` 连接、会话、KV 或应用数据。不要复制旧数据库和凭证到新目录。旧版数据保留不等于可直接由新版使用。

使用相同 `plugins-v1` 数据格式的开发候选可保留数据，运行组合在后台空闲后切换。看板早期开发 schema 1 不迁移到 schema 2；正常更新或重装 schema 2 插件保留同格式业务数据。

## 安装官方插件

在应用中心刷新官方市场并安装 **看板 0.1.0** 或 **模型目录 0.1.0**，两者均无需模型 API Key。看板提供本地任务管理，模型目录浏览 models.dev 规格与报价，不产生模型调用，也不自动配置连接。

[官方插件仓库](https://github.com/semicoder-dev/fn-plugins) 使用 GitHub Releases 分发归档；[schema 3 市场索引](https://raw.githubusercontent.com/semicoder-dev/fn-plugins/main/marketplace.json) 提供固定下载地址、摘要与兼容版本。旧译文、随手记、网络搜索不在本版市场，历史 schema 2 索引仅供旧宿主使用。

安装先下载、校验并准备新的运行组合，再等待任务空闲后重启后台；窗口保留并重连。插件是可信本机代码，只安装信任的来源。接着阅读[第一次使用](/fn/docs/getting-started)和[插件管理](/fn/docs/plugins)。

## SDK 与插件开发

公开 SDK **0.5.0** 面向 fn 0.6，可以独立安装：

```sh
bun add --dev --exact @semicoder/fn-sdk@0.5.0
```

应用菜单可安装 `fn` CLI，使用 `fn plugin check/build/pack`。后端可直接运行 ESM TypeScript，前端须构建并共享宿主 React、Cordis 和 fn-ui。SDK 不再提供旧 UI、KV、网页搜索或聚合应用 API。

官方仓库根目录可独立执行 `bun install --frozen-lockfile --ignore-scripts`、`bun run check`、`bun run test`、`bun run pack`。其中 SDK 来自公开 npm，尚未独立发布的宿主契约由仓库固定归档提供，详见[插件开发](/fn/docs/plugin-development)。

## 从源码构建

[fn v0.6.0 源码](https://github.com/semicoder-dev/fn/tree/v0.6.0) 需要 Bun 1.4.3 和 Swift 6 / Xcode 工具链。在仓库根执行：

```sh
bun install --frozen-lockfile --ignore-scripts
bun run check
bun run build
```

产物为 `dist/fn.app`，本机构建没有官方发行的签名公证证明。FIA 在同仓 `framework/fia/` 构建；公开 FIA npm 0.18.0 与共仓源码不是同一产物，独立 FIA npm 发布继续暂停。

日常体验使用 `bun run dev`（fn Dev）或 `bun run run`（fn Preview），分别使用 `.fia/dev/data/com.semicoder.fn`、`.fia/preview/data/com.semicoder.fn`，禁用正式更新源。直接打开 `dist/fn.app` 使用正式应用数据目录。

## 更新与聊天边界

应用更新源为 `https://semicoder-dev.github.io/fn/updates/latest.json`。启动时及每 24 小时检查，安装前需确认；后台忙碌时推迟，原生运行时改变需要完整 DMG。插件市场与应用更新源独立管理。

Chat 用途仍在设计中，0.6.0 不发行会话页、默认助手、卡片或 iCloud 聊天同步。源码仅保留可选实验组合 `bun run dev --experimental-chat`，实验归档不能用于正式发行。

手机内测仍为 **0.3.0（4）**，对应旧桌面 Chat v3，不与 fn 0.6 同步；本次没有新手机版本或公开邀请。通用 Agent、Session、模型与工具仍可由桌面插件组合使用。
