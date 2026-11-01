---
id: "docs-fn-plugin-styling"
slug: "plugin-styling"
title: "主题与样式"
description: "CSS Modules、语义变量与本地化。"
date: "2026-10-31"
---

使用 CSS Modules，复用 Fn UI 的语义变量和组件。Bun 构建负责 CSS Modules 合并，开发与生产使用同一路径。不要引入额外 CSS 框架或依赖宿主内部样式类。

主题为 light/dark/system，语言为 en/zh-CN。ApplicationProvider 接收宿主偏好；业务文案写入 locales，使用 useI18n。应用名称与摘要文本也可引用词条，用户内容不翻译。

检查明暗对比、窄窗口溢出、键盘可达、菜单弹窗焦点恢复与 prefers-reduced-motion。摘要采用固定区域和预定义语义样式，应用不指定尺寸、不提供行 CSS。
