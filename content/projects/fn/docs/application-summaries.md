---
id: "docs-fn-application-summaries"
slug: "application-summaries"
title: "应用列表与摘要"
description: "每个应用一行，固定区域中的动态摘要。适用于 Fn 0.8、SDK 0.7 / UI 0.2 的 hostApi 2。"
date: "2026-11-01"
---

# 应用列表与动态摘要

本文对应 Fn 0.8、SDK 0.7.1、UI 0.2.1 的 hostApi 2。应用通过结构化摘要提供首页内容，宿主统一渲染。

## 首页

每个已安装应用自动显示一行，点击或 Enter 进入应用；搜索直接过滤列表。排序支持拖动、上下按钮和聚焦行后的 Alt+↑/↓，不再添加或隐藏独立小组件。应用中心仍负责安装、更新和卸载，Dock 保留应用、通知、设置和运行应用。

宿主统一显示图标、名称、运行状态、更新时间和管理菜单。应用自定义摘要内容，不能改变行框架、运行状态或注入脚本。宽窗横向排列，窄窗摘要下移、指标换行并隐藏趋势图；文字最多显示两行，补充文字一行。

## 清单与发布

```json
{
  "hostApi": 2,
  "uses": ["application.summary"],
  "contributes": { "summary": { "initial": "summary/initial.json" } }
}
```

以上为清单片段；动态摘要要求 backend 入口。summary 声明和 initial 文件均可省略，不提供动态摘要时无需声明该能力。包内初始摘要在构建及安装时校验。

```ts
await host.application.setSummary({
  version: 1,
  text: "最近保存的笔记",
  detail: "内容摘要",
  metrics: [{ value: 12, label: { key: "count" } }],
  badges: [{ text: "待处理", tone: "warning" }],
  progress: { value: 0.75 },
  sparkline: { values: [1, 3, 2, 5, 4, 7], tone: "success" },
});
```

每次完整替换唯一摘要；省略字段清除旧字段。`setSummary(null)` 回退到包内初始摘要或应用描述。metrics 最多三个、badges 最多两个、progress 为 0–1、sparkline 为 2–120 个有限数；快照最多 16 KiB UTF-8 JSON，不接受任意布局、HTML、CSS 或前端代码。文本沿用字符串或 `{key,args}`。

摘要按实例独立保存，只接受当前后台运行代的更新；写入失败保留最后有效内容。关闭后显示缓存及停止状态，重启不会自动启动应用。更新应用包重置派生摘要，保留业务数据库。

## 页面刷新与预览

```ts
await host.application.notifyDataChanged();
// 页面订阅，重连后也重新读取业务数据
view.addEventListener("dataChanged", refresh);
view.addEventListener("reconnected", refresh);
```

业务通知和摘要更新彼此独立。不要让摘要更新失败覆盖已经提交的业务结果；事件只用于本应用当前运行代的视图刷新，不提供持久投递。

```sh
fn plugin summary summary/initial.json
fn plugin summary summary/initial.json --preview
```

预览复用实际应用行，支持明暗、语言、运行状态和文件变更刷新。正式应用精确依赖公开 SDK 0.7.1 / UI 0.2.1，发布前在仓库外检查与打包。开发联调可以使用带哈希的本地归档，发行时必须恢复公开依赖。

旧小组件 API、布局字段及预览命令不保留兼容。旧应用包可管理但不能启动，需安装按新 SDK 重建的应用。先更新 Fn，再在应用中心更新应用；更新保留业务数据。
