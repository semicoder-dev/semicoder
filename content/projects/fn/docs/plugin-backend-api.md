---
id: "docs-fn-plugin-backend-api"
slug: "plugin-backend-api"
title: "后台 SDK"
description: "defineBackend、数据目录与聊天。"
date: "2026-10-31"
---

`defineBackend({api,setup({host})})` 注册后台。setup 返回 handlers、可选 chat 和 stop。方法输入输出由 Zod 契约验证；流式方法返回 AsyncIterable，CallContext 包含 signal、requestId 与可选 viewId。

host.paths 提供 data/cache/logs。应用使用 Bun SQLite 或文件保存业务数据，不写入不可变安装目录。host.models 提供目录、默认模型和 Responses；host.widgets.publish 发布完整快照；host.ui 提供定向 Toast/确认。

chat 的键匹配 manifest 聊天 ID。处理器取得 message、signal、reply，调用 `reply.complete({text})` 持久提交最终回复。手机入口没有 viewId，不能等待页面确认弹窗。

stop 关闭数据库、定时器和业务资源。宿主先取消请求，再给清理期限，最后终止进程组。不要把进程独立误认为系统权限沙箱。
