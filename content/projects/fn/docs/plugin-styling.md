---
id: "docs-fn-plugin-styling"
slug: "plugin-styling"
title: "样式与主题"
description: "CSS Modules、语义 token 和共享资源生命周期。"
date: "2026-10-31"
---

适用于 fn 0.6。样式采用 CSS Modules，使用宿主原版 dsh `--dsw-*` 语义变量；不引入 Tailwind、Base UI、旧 SDK 主题或第二份 UI 样式。

```tsx
import styles from "./style.module.css";
export function Panel() {
  return <section className={styles.panel}>内容</section>;
}
```

需要共享布局类时可使用 fn-ui 的 `classes(local, ...values)`，优先解析局部和共享 layout 模块。具体 token 从固定版本的主题中选择，不猜测不存在的变量。

客户端构建将样式关联到插件生命周期；卸载清理样式和注册，不向 document 写永久全局规则。禁止全局覆盖 button、input、dialog 等控件。

主题仅浅色、深色、跟随系统。业务数据颜色（例如看板标签）可使用有明确含义的独立色板，但应保证明暗可读性。验收包含窄窗、键盘焦点、弹层材质与减少动态效果。
