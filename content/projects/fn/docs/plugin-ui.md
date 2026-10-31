---
id: "docs-fn-plugin-ui"
slug: "plugin-ui"
title: "共享 UI"
description: "使用宿主共享的原版 dsh 控件和组合组件。"
date: "2026-10-31"
---

fn 0.6 的 UI 从 `@semicoder/fn-ui` 导入，SDK 0.5 不再提供 `./ui`、主题和 Tailwind 导出。插件声明 fn-ui peer，由宿主提供同一份控件与 React。

## 控件

共享导出包含 Button、Input、Checkbox、Switch、Menu、Modal、Tooltip、Tag、SegmentedControl 和图标；Select、Textarea、Notice、PageHeader、Surface、SettingsSection、SettingsRow 等组合组件同样由 fn-ui 提供。准确 props 以固定契约的类型为准。

```tsx
import { Button, Select, PageHeader } from "@semicoder/fn-ui";
<PageHeader title="示例" actions={<Button onClick={save}>保存</Button>} />;
<Select
  label="状态"
  value={status}
  onValueChange={setStatus}
  options={[
    { value: "todo", label: "待办" },
    { value: "done", label: "完成" },
  ]}
/>;
```

使用语义清楚的标签，图标按钮提供可访问名称。列表、表单和空状态由业务页面组合，不复制一份控件实现。

## 布局与主题

业务页面采用 CSS Modules，原版主题由 ui-base/ui-theme 统一加载。只提供浅色、深色和跟随系统；不恢复旧自定义颜色主题。布局适配窄窗口，避免页面和内部列表同时产生不必要的滚动条。

官方看板展示了二级页面、密集工具栏、日期、标签和拖动交互；模型目录展示了筛选、分页和详情。见[弹层交互](/fn/docs/plugin-ui-overlays)和[样式规则](/fn/docs/plugin-styling)。
