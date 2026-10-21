---
id: "docs-fn-automation"
slug: "automation"
title: "CLI 与自动化"
description: "fn 0.5 的应用 CLI、分页查询和持久会话自动化。"
date: "2026-10-21"
---

适用于 fn 0.5.0。使用随宿主提供的 FIA CLI，不启动额外后台服务器。开发命令前缀为 `fn-dev`，预览为 `fn-preview`，正式版为 `fn`。

## 查询状态

```sh
fn call apps.list --json '{}'
fn call connections.list --json '{"offset":0,"limit":50}'
fn call models.saved --json '{"offset":0,"limit":50}'
fn call sessions.list --json '{"offset":0,"limit":50}'
```

真实操作及参数以宿主命令帮助和共享 API 为准。列表可能分页，需处理 `nextOffset/hasMore`；模型目录和标题等无 Agent 调用也必须显式选择连接。业务方法通过 `apps.invoke` 调用，必须提供应用 ID、方法名和输入。

## 页面与任务

```sh
fn open --browser
fn application create ./my-app
fn application check ./my-app
fn application build ./my-app
fn application pack ./my-app
```

浏览器授权属于当前实例，后台重启后重新连接。页面断开不停止后台任务，任务取消和审批回复均使用显式命令。不要将临时流式事件当作唯一历史；重连从持久 Session 投影和游标补齐。

旧 `plugins.*`、`runs.*` 和 `plugin create` 属于旧架构，不适用于新宿主。新数据目录独立，脚本不应直接操作旧数据库或复制旧凭证。
