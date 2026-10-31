---
id: "docs-fn-plugin-client-api"
slug: "plugin-client-api"
title: "前端 API"
description: "类型化客户端、插槽与导航。"
date: "2026-10-31"
---

客户端是 Cordis 插件，导出 `inject` 和 `apply`。SDK 0.5 的 `./client` 提供 `client`、`readAllPages`，不再提供聚合 clientApp。

## 调用后台

将契约放在共享 `contract.ts`：

```ts
import { client } from "@semicoder/fn-sdk/client";
import contract from "./contract.ts";
const remote = client(ctx, "example", contract);
const status = await remote.call("read", {});
```

具体调用语法以契约生成的类型为准。前端依赖 `platform` 和 `api:example`，避免服务尚未注册就挂载页面。分页通过 `readAllPages` 或响应游标读取；订阅与异步任务应在 effect 清理时释放。

## 注册界面

```tsx
import type { Context } from "@deepseek-ai/cordis";
import type {} from "@semicoder/fn-ui-layout/contract";
export const inject = ["slots", "layoutSlots"];
export function apply(ctx: Context) {
  ctx.slots.register({ name: "fn.status", id: "example" }, () => <span>示例已连接</span>);
}
```

布局提供 `fn.sidebar.top`、`fn.sidebar.bottom`、`fn.toolbar`、`fn.status`、`fn.overlay` 列表插槽，以及按 key 选择的 `fn.main`。插槽类型来自 dsh slots 和布局 contract。

完整页面可使用 `@semicoder/fn-ui-navigation/view` 的 `NavigationButton`、`useNavigation`，注入 `navigation` 后以路径进入对应 `fn.main` 页面。参考官方看板的首页/详情路径；不要创建宿主之外的路由系统。
