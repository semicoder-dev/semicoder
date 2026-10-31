---
id: "docs-fn-plugin-ui-overlays"
slug: "plugin-ui-overlays"
title: "弹窗与通知"
description: "视图定向、关闭取消与焦点恢复。"
date: "2026-10-31"
---

`view.ui.confirm(text)` 返回 `Promise<boolean>`，使用标准 Fn 确认界面；`view.ui.toast(text)` 显示当前视图通知。后台传 CallContext.viewId 定向通知或确认，不能假设每个任务都有页面。

视图消失时未决确认取消。关闭应用存在执行任务或未保存内容时由宿主协调一次确认。应用编辑器使用 `useUnsavedChanges` 登记；嵌套编辑区域分别登记，避免子弹窗关闭清除父编辑状态。

Modal 支持键盘 Escape、焦点约束与恢复。复杂编辑可使用应用 Modal，但数据保存和脏状态仍由业务管理。
