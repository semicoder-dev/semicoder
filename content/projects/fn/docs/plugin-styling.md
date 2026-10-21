---
id: "docs-fn-plugin-styling"
slug: "plugin-styling"
title: "样式与主题"
description: "Cordis 页面共享主题、组件和明确管理的应用样式。"
date: "2026-10-21"
---

适用于 fn 0.5.0 / SDK 0.4.0。宿主提供主题、公共组件 CSS 和 reset，应用负责自己的业务布局。

## 共享 UI

从 `@semicoder/fn-sdk/ui` 导入 Button、Input、Dialog 等组件。浏览器构建将 SDK UI、React 和 Cordis 保持为宿主共享模块，不能打包第二份运行时。

## 应用样式

当前 Cordis 构建入口只输出客户端 JavaScript，SDK 不自动注入应用独立 CSS 文件。可以使用共享组件及内联 React style，或由应用明确管理样式注入并用 Cordis effect 清理。不要沿用旧构建器的 CSS Modules/Tailwind 自动输出假设。

共享主题变量可在样式中使用，例如 `var(--m-bg)`、`var(--m-text)`、`var(--m-line)`。通过组件继承浅色、深色、跟随系统与用户配色，不硬编码整页背景。避免修改 body 或其他应用节点；可信同页面执行不等于样式隔离沙箱。

## 浮层和隐藏页面

宿主已经提供 UIProvider 和当前页面可见性，普通应用不嵌套 Provider。keepAlive 页面隐藏时，公共浮层关闭；自定义 DOM、事件和计时器仍需自行清理。

## SVG 图标

在 `src/icon.svg` 放置不超过 8 KiB 的单色 SVG，并声明 `fn.iconSvg: "dist/icon.svg"`。构建器会随 dist 原子更新。宿主以 mask/图片显示并跟随前景色，不把 SVG 内容插入页面 DOM。
