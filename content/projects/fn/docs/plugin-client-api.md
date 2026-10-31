---
id: "docs-fn-plugin-client-api"
slug: "plugin-client-api"
title: "页面 SDK"
description: "类型化调用、模型、取消与窗口。"
date: "2026-10-31"
---

通过 `connectView(contract)` 创建页面客户端，并用 `ApplicationProvider` 包裹页面。只在 Fn 提供的页面入口中连接，不自行拼接后台端口。

- `view.call(name,input,{signal})` 调用自身后台普通方法。
- `view.stream(name,input,{signal})` 返回异步流，取消或断开视图会传递到后台。
- `view.models.list/default/responses` 使用宿主模型；Responses 连接参数在 options。
- `view.ui.toast(text)` 与 `view.ui.confirm(text)` 定向当前视图。
- `view.application.close/minimize/maximize` 操作当前应用；编辑器调用 `useUnsavedChanges(view,dirty)`。
- `useI18n(view)` 提供 locale/t；词条在应用包中，主题与语言由宿主下发。

关闭标签页只断开视图，显式关闭应用才停止后台。停止或更新后的视图失效，不能继续使用陈旧凭证。详情与可运行示例见[应用开发](/fn/docs/plugin-development)。
