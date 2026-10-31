---
id: "docs-fn-plugin-backend-api"
slug: "plugin-backend-api"
title: "后端 API"
description: "服务注入、动态命名空间与插件独立持久化。"
date: "2026-10-31"
---

SDK 0.5 不提供集中应用 Context。后端直接使用 Cordis `Context`，模型、任务、Session、RPC 等服务的类型来自所属插件。

## 定义与注册接口

```ts
import type { Context } from "@deepseek-ai/cordis";
import { defineAPI, implementAPI, z } from "@semicoder/fia/api";
import type {} from "@semicoder/fn-rpc";

const contract = defineAPI({
  methods: {
    read: { description: "Read status", input: z.object({}), output: z.string() },
  },
});
export const inject = ["rpc"];
export function apply(ctx: Context) {
  ctx.rpc.register("example", implementAPI(contract, { read: () => "ready" }));
}
```

真实项目将 contract 放在浏览器安全的独立文件并导出，服务包声明 `@semicoder/fia`、`@semicoder/fn-rpc` 等实际 peers。命名空间应按实例策略选择，避免不同实例冲突。注册绑定 Cordis 生命周期，卸载关闭入口并取消在途调用。

## 持久化与任务

注入 `host` 后，以 `ctx.host.dataDirectory` 下的插件专属目录保存业务 SQLite 或文件；插件负责 schema 和一致性。看板示例使用 `fn-kanban/kanban-v2.sqlite`。没有公开全局 KV，也没有 SDK 网页搜索接口。

后台工作通过 `fn-activity` 登记，例如 `ctx.activity.run(async () => ...)`，使运行组合在忙碌时等待。数据库、监听器和网络请求通过 `ctx.effect` 注册清理；窗口关闭和后台任务取消是不同生命周期。

模型、Session、Agent 和工具契约分别从对应 fn 插件与原版 dsh 获取。插件是可信本地代码，Cordis scope 用于服务组织，不是进程安全沙箱。
