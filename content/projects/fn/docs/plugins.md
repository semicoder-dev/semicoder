---
id: "docs-fn-plugins"
slug: "plugins"
title: "安装与管理插件"
description: "通过应用中心或 CLI 安装预构建插件，了解启停、升级和数据保留。"
date: "2026-10-16"
---

> 适用于 fn 0.4.5 / SDK 0.3.5，manifest 为 0.3。先按[安装指南](/fn/docs/installation)安装 fn；旧名称插件和旧应用数据不迁移。

本页适用于 fn 0.4.5。应用不内置插件；官方市场已上架译文和随手记 0.1.2，详见[安装步骤](/fn/docs/installation#安装官方插件)。

## 从插件市场安装

首次启动进入「应用中心 → 插件市场」。官方市场提供「译文」和「随手记」，点击安装，任务成功后即可打开。随手记不需要模型，适合先体验安装与本地保存。

进入市场会检查更新；有新版时逐个点击「更新」，不会自动安装。支持搜索、市场筛选、刷新及来源管理。安装与更新需要联网，成功读取的索引保留离线缓存和更新时间；一个来源失败不影响已安装插件运行。

## 管理市场源

默认配置官方市场，维护源码在 [fn-plugins](https://github.com/semicoder-dev/fn-plugins)。在「管理市场源」添加名称与 HTTPS 索引地址，也可重命名、启停和删除。官方源同样可以停用或删除，重启后不会恢复；修改地址请删除后重新添加。

不同源的同 ID 条目分别显示来源。已安装插件固定使用原市场更新，不跨来源替换；来源被删后插件仍可运行。手动安装不自动关联市场，如需市场更新，先卸载再从选定市场安装相同 ID。

## 从地址安装

点击「从地址安装」，输入来源或「选择本地包」，再安装。

| 来源   | 输入形式                  | 要求                                 |
| ------ | ------------------------- | ------------------------------------ |
| 本地包 | `.tgz` 绝对路径或文件选择 | 使用 `fn plugin pack` 生成的完整归档 |
| npm    | 包名或带版本包名          | 包已存在并预构建                     |
| Git    | `git+https://…`           | 仓库含可运行 `dist`，安装时不编译    |

官方插件通过 GitHub Releases 分发，不是公开 npm 插件包。npm / Git 入口保留，真实第三方远端插件未逐一验收。

## 更新、启停与卸载

「已安装」页提供打开、启停、卸载、任务和错误反馈。更新只接受更高的稳定 `x.y.z` 版本，校验后切换，保留启停状态、KV 与历史。激活或保存失败恢复旧版本；请先保存草稿，更新会释放旧页面。

正在运行的模型任务或插件调用会阻止替换、停用和卸载。卸载保留 KV 和历史，重新安装相同 ID 可以继续读取。手动插件通过 `plugins.update` 显式指定新版来源更新。

## 兼容与信任

manifest 使用 `fn` 字段，`sdkVersion` 为 `"0.3"`。仅接受按当前 fn SDK 构建的插件，不提供旧插件兼容。

市场安装先校验归档 SHA-256，再核对包名、ID、版本、SDK 和资源。插件是能执行本机代码的可信扩展，禁用安装脚本不等于沙箱，只安装信任的来源。

## CLI

```bash
fn call marketplaces.refresh --json '{}'
fn call marketplaces.catalog --json '{"offset":0,"limit":20}'
fn call plugins.installFromMarket --json '{"marketId":"official","pluginId":"quick-notes"}'
fn call plugins.jobs --json '{}'
fn call plugins.update --json '{"pluginId":"quick-notes"}'
fn call plugins.install --json '{"source":"/absolute/path/plugin.tgz"}'
```

安装更新返回任务，接受任务不表示成功，继续查询 `plugins.jobs` 终态。开发项目使用 `bun run agent` 代替 `fn`。重连后重新读取市场、插件和任务快照。

## 发布市场

市场使用静态 JSON v1，包含 `schemaVersion/name/plugins`；条目包含 `id/packageName/name/description/icon/color/version/sdkVersion/url/sha256`。只列当前稳定版本，索引与归档使用 HTTPS，无需专门市场服务。

参考 [fn-plugins](https://github.com/semicoder-dev/fn-plugins) 及[宿主市场协议](https://github.com/semicoder-dev/fn/blob/v0.4.5/docs/plugin-marketplace.md)。官方插件分别打标签、上传归档，验收公开下载哈希后通过索引 PR 上架。

制作自己的插件可从[第一个插件](/fn/docs/plugin-development)开始；归档、市场索引与更新规则见[构建与分发](/fn/docs/plugin-distribution)。

## 开发中的网络搜索与 fn Chat

本地开发插件「网络搜索」使用宿主 Agent loop 搜索、阅读网页并总结，页面支持模型选择、历史、追问、停止和实际来源列表。每轮按宿主当前时间检索，只展示本轮取得的来源，标注已读正文、部分正文或仅搜索摘要。检索过程显示查询和结果数量；工具失败、没有来源时在答案下方提示。空结果会尝试放宽筛选，动态页面没有有效内容时换来源，仍无法核实时明确说明。

在已配置 Chat v2 同步的开发宿主中安装后，fn Chat 会出现「网络搜索」机器人。直接发送问题，首次可通过原生卡片选择模型；后续文字继续当前话题。`/新话题` 开始新会话，`/模型` 重新选择模型，`/帮助` 查看说明。搜索进度在处理状态中显示，答案和来源完成后回复。Chat 按会话串行处理，中止任务请在宿主插件页面点击「停止」。

此插件 0.1.0、Agent 和 Chat v2 接口均属于未公开开发范围，公开 fn 0.4.5 / SDK 0.3.5 不支持，正式市场仍只有译文与随手记。开发归档需从相同源码阶段构建，手动安装到兼容开发宿主；没有新增手机协议或 CloudKit schema。开发宿主已完成真实模型的天气与新闻基础验收，不代表所有问题、来源和手机多端/推送均已验证。更新研究策略后，已有 Chat 话题发送 `/新话题` 使用新策略，原历史保留。
