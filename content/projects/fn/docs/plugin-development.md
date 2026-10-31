---
id: "docs-fn-plugin-development"
slug: "plugin-development"
title: "插件开发"
description: "使用 SDK 0.5 创建普通 Cordis 后端和前端插件。"
date: "2026-10-31"
---

适用于 fn 0.6 / SDK 0.5。插件是普通 ESM npm 包，可以只有后端、只有前端，也可以同时提供两者。旧 SDK 0.4 聚合应用 API 不兼容。

## 开始开发

SDK 负责构建和类型化客户端；最完整的可复现示例是[官方插件仓库](https://github.com/semicoder-dev/fn-plugins)中的 `plugins/kanban` 和 `plugins/models-dev`。克隆仓库后在根目录执行：

```sh
bun install --frozen-lockfile --ignore-scripts
bun run check
bun run test
bun run pack
```

SDK 精确依赖公开 `@semicoder/fn-sdk@0.5.0`。宿主 UI、RPC 等尚未独立发布的契约由该仓库 `vendor/manifest.json` 固定归档及 SHA-256 提供；不能用 npm 上同版本号的其他包替代。宿主不依赖此仓库构建。

## 目录与入口

后端 `exports["."]` 可指向 TypeScript，由 Bun 直接运行。前端源码通过 `fn.clientSource` 指定，构建后 `exports["./client"]` 指向模块 factory。浏览器安全 API 类型放在独立 `./contract` 导出，不能导入后台存储或凭证模块。

声明每个使用的共享依赖为精确 peer，保持 Cordis、React、FIA、SDK 和 fn-ui 单实例。后台用 `inject` 声明服务，通过 Cordis effect 清理资源；前端注册布局插槽，不修改集中路由。

## 检查和安装

```sh
fn plugin check ./my-plugin
fn plugin build ./my-plugin
fn plugin pack ./my-plugin
```

也可从 `@semicoder/fn-sdk/build` 调用 `inspectPlugin`、`buildPlugin`、`packPlugin`。把输出归档安装到 fn Dev 或隔离 Preview，验证菜单/弹窗焦点、明暗、窄窗、重启持久化及卸载清理，再公开归档。

继续阅读[包配置](/fn/docs/plugin-manifest)、[后端 API](/fn/docs/plugin-backend-api)、[前端 API](/fn/docs/plugin-client-api)与[分发](/fn/docs/plugin-distribution)。

## 热更新生命周期

fn 0.6.1 保留 SDK 0.5.0，普通外部插件支持局部更新。将注册、事件监听、定时器和数据库连接绑定 Cordis effect，在停用或替换时完整释放；避免模块顶层启动长期副作用。插件持久数据自行管理，被替换组件的临时状态不保证保留。新增 hotReload 声明的 SDK 构建支持尚未发布，当前 SDK 0.5.0 不接受此字段。
