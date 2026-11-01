---
id: "docs-fn-plugin-ui"
slug: "plugin-ui"
title: "标准 UI"
description: "Fn UI 与宿主主题语言。"
date: "2026-10-31"
---

页面使用自有 `@semicoder/fn-ui`，包括 Button、Input、Textarea、Select、Checkbox、Menu、Modal、Toast、Notice、Surface、图标和布局。通过 ApplicationProvider 统一主题、语言和公共弹窗。

页面可按业务设计，但应沿用组件尺寸、间距和语义颜色；遵守键盘、焦点恢复、窄窗与减少动态效果。全局 Toast 和确认默认只显示在发起视图；后台无视图通知进入宿主通知入口。

应用摘要不渲染应用 React。使用[ApplicationSummary](/fn/docs/application-summaries)，宿主和预览器共享 `@semicoder/fn-ui/summary` renderer，不接受行内业务按钮或自定义 CSS。

配色统一由 Fn UI 的 `src/theme.css` 定义，参考 dsh 的明暗设计：浅色白底、深色近黑底、中性主按钮、蓝色焦点和强调色。宿主与应用页面都消费 `--fn-*` 语义变量；背景、文字、边框、输入和弹窗颜色随主题切换，业务页面不要重定义全局主题。

| 用途                 | 变量                                               |
| -------------------- | -------------------------------------------------- |
| 页面 / 卡片 / 浮层   | --fn-bg、--fn-surface、--fn-surface-raised         |
| 文字层级             | --fn-text、--fn-secondary、--fn-muted              |
| 主按钮 / 焦点 / 强调 | --fn-accent、--fn-on-accent、--fn-focus、--fn-info |
| 边框 / 分隔线        | --fn-border、--fn-line                             |

组件尺寸和交互由 Fn UI 维护，宿主布局由自身 CSS Modules 管理。配色映射不需要 dsh 运行依赖；应用自包含打包 Fn UI，更新主题后应重新构建应用包。

UI 0.2.1 中，Input、Textarea、Button 和 Select 复用原边框显示焦点；菜单项和无边框弹窗关闭按钮使用内部细线。鼠标与键盘焦点保持可辨识，控件尺寸不随聚焦变化。
