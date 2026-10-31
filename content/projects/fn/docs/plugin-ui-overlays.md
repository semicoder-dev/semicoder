---
id: "docs-fn-plugin-ui-overlays"
slug: "plugin-ui-overlays"
title: "菜单与弹窗"
description: "沿用原版 Menu 和 Modal 的焦点、键盘与层级行为。"
date: "2026-10-31"
---

fn 0.6 使用 `@semicoder/fn-ui` 的原版 dsh Menu、Modal、Tooltip。旧 SDK Base UI 的 Root/Trigger/Content 组合不适用于本版。

## 菜单

Menu 通过 `anchor`、`items`、`open`、`onClose` 和 `onSelect` 组合。使用受控状态，按需设置 `portal`、`autoFocus` 和 `selectedId`；准确属性以固定契约类型为准。共享 Select 已封装单选字段的 Menu 和焦点处理。

WebKit 点击按钮不一定自动聚焦，打开菜单时应保留可恢复的触发元素。不要用手写全局监听替代控件键盘行为，也不要靠任意高 z-index 掩盖层级问题。

## 弹窗

使用 Modal 提供标题、内容与操作；提交、取消和关闭保持一致。异步保存期间显示可理解的状态，错误留在用户仍可操作的位置。嵌套菜单应正确处理 Escape：关闭当前弹层后把焦点交回合适的触发元素。

Modal 已提供标题与正文间距。业务表单用局部容器的 `gap` 统一字段间隔，字段与提示段落显式设置 `margin`，避免再次叠加顶部留白或 flex 子项外边距；空列表没有内容时不保留占位。初始输入用 `data-modal-autofocus`，让 Modal 正确记录关闭后应返回的触发元素。

## 验收

检查鼠标与键盘开启、方向键、Enter、Escape、Tab、点击外部关闭和焦点返回；同时验证明暗、窄窗口、内容滚动、菜单阴影及模糊材质。CSS Modules 只处理业务布局，不覆盖原版控件的全局选择器。官方看板标签编辑与卡片详情可作完整参考。
