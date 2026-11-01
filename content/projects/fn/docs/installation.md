---
id: "docs-fn-installation"
slug: "installation"
title: "安装 Fn"
description: "下载 macOS 桌面平台并安装应用。"
date: "2026-11-01"
---

## macOS 安装

Fn 0.8.2（build 18）面向 macOS 14+、Apple Silicon。新用户下载完整安装包，打开 DMG，将 **Fn.app** 拖入 Applications，再从「应用程序」启动。

[下载 Fn 0.8.2](https://github.com/semicoder-dev/fn/releases/download/v0.8.2/Fn-0.8.2-18-mac-arm64.dmg) · [发行说明与校验文件](https://github.com/semicoder-dev/fn/releases/tag/v0.8.2)

| 当前情况        | 更新方式                                                                         |
| --------------- | -------------------------------------------------------------------------------- |
| 已安装 Fn 0.8.1 | 在 macOS 菜单栏的 Fn 菜单选择「检查更新…」，下载后确认更新，无需重新下载安装包。 |
| 已安装 Fn 0.8.0 | 用完整 DMG 一次升级到 0.8.2，无需先安装 0.8.1。                                  |
| 尚未安装 Fn     | 下载上方 DMG 安装。                                                              |

现有应用、模型连接、业务数据、摘要和偏好保留，无需重装独立应用。安装包内置 Bun，使用 Fn 无需自行安装开发工具。桌面正式版暂不提供手机连接。

## 应用内更新

0.8.2 复用公开 0.8.1 的固定运行时。普通页面与 TypeScript 更新可在应用内完成：下载代码，确认后重启后台并刷新界面，保留业务数据；请先保存未完成的编辑。更新失败时回到上一份可用代码，业务数据不随代码回退。

原生能力、Bun、权限或固定资源变化仍需完整安装包。每版都提供 DMG 供新用户安装，提供 DMG 本身不代表已有用户必须重装。

独立应用在「应用 → 检查更新」中更新，与 Fn 菜单中的桌面更新分开。当前看板为 0.3.1，翻译、笔记和模型目录为 0.3.0；SDK/UI 和应用市场不随本次桌面补丁改版。已使用 Fn 0.8 的用户只更新看板，也能获得看板异常边框修复。

## 安装应用

点击底部 Dock 的「应用」，从官方市场安装翻译、笔记、看板和模型目录。也可添加自定义 HTTPS 市场或通过「本地安装」选择 `.fn.tgz`。

安装后从首页应用行打开应用。笔记与看板可直接使用，模型目录联网查询无需 API Key；翻译需在「设置 → 模型」登录 ChatGPT 并选择默认模型。

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
