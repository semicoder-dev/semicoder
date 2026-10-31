---
id: "docs-fn-cordis-preview"
slug: "cordis-preview"
title: "全插件化架构"
description: "fn 0.6 的最小宿主、Cordis 组合与动态 API。"
date: "2026-10-31"
---

fn 0.6 使用 Bun/FIA 最小宿主。模型、连接、Session、工具、应用中心、设置和布局均由 `@semicoder/fn-*` 插件提供。宿主负责启动、原生能力、配置、运行目录和故障恢复。

## 插件组合

`profiles/default.json` 通过 bundle 和 patch 组合默认产品。后端采用原版 Cordis Loader/Include，前端采用客户端模块、Slots 和 renderer。包名与实例 ID 分开；配置经过校验后准备新运行代，等所有登记任务空闲再切换。

共享内核、React、SDK 和 UI 保持单实例。安装插件只准备变化部分及私有依赖，完整依赖图仍须通过校验。历史运行目录保留用于恢复，不把运行目录回滚当作数据库迁移。

## 服务与 API

服务声明自己的 contract，`fn-rpc` 将其注册到 FIA 动态命名空间。没有集中业务 Bridge 或万能应用对象；公开 SDK 不提供旧 KV、网页搜索、Agent 或 Session 副本。

插件持有自己的业务数据。新数据根为 `Backend/plugins-v1/`，不读取、迁移或删除旧 `cordis-v1` 数据。SDK 0.5 和 schema 3 市场与旧插件不兼容。

## 本阶段边界

默认首页为应用中心。通用 Agent、Session、模型和工具保留供插件组合使用；Chat 页面、默认助手、卡片和 iCloud 聊天同步不随 0.6.0 发行。

聊天代码仅作为源码中的 `profiles/chat-experimental.json` 开发组合，使用 `bun run dev --experimental-chat` 显式启用。正式构建和更新会拒绝实验 Chat 归档；现有手机内测客户端不与 0.6 同步。
