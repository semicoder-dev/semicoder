---
id: "docs-fn-plugin-backend-api"
slug: "plugin-backend-api"
title: "后端 API"
description: "Cordis 后端、工具审批、持久会话与跨端自定义卡片。"
date: "2026-10-21"
---

适用于 fn 0.5.0 / SDK 0.4.0。后端是标准 Cordis 插件，在 `inject = ["app"]` 后使用 `ctx.app`。Bun 可直接执行 TypeScript，插件与宿主同进程，属于可信本地扩展。

## 应用能力

| 接口                              | 用途                                   |
| --------------------------------- | -------------------------------------- |
| `info`                            | 应用身份                               |
| `kv.get/set/delete/list`          | 同步、隔离的 JSON KV，单值最多 256 KiB |
| `method(name,handler)`            | 注册 JSON 业务方法，应用校验输入       |
| `chat(endpoint)`                  | 贡献可拥有多个 Session 的 Chat 入口    |
| `useTools(names)`                 | 为当前应用启用官方工具                 |
| `createSession(selection)`        | 以显式连接/模型创建 Session            |
| `send(sessionId,text,requestId?)` | 持久接收请求，后台继续生成             |
| `agent(sessionId)`                | 获取本应用 Agent                       |

`createSession` 指定 `connectionId/modelId`，可附带 `endpointId/policy/cwd/title`。同会话固定连接，换账号或端点新建连接和 Session。长任务返回 Session ID，不阻塞 RPC 等待整个生成。工具、提示词、监听器随应用/Agent 作用域组织，使用 Cordis effect 释放后台资源。

## 工具与持久化

官方工具包括文件读写、进程执行、`web_search` 和 `web_fetch`。读取/搜索在启用范围内执行；写入、进程和外部副作用默认需要批准，应用可显式选择自动执行。取消传播到请求和子进程。模型请求和工具副作用前由 checkpoint policy 检查持久化；失败阻止后续执行。

Agent 历史以 dsh Session 为准。重启恢复展示，中断的副作用不会自动重放。网页搜索可能被验证码或网络限制阻断，应用应展示实际错误，不把失败解释为“没有结果”。

## Session 自定义卡片

```ts
ctx.app.cards.action("save", ({ cardId, values }) => {
  ctx.app.kv.set(`note:${cardId}`, { text: values.text });
});
ctx.app.cards.create(sessionId, {
  key: "note-1",
  handler: "save",
  card: {
    version: 1,
    template: "form",
    title: "保存笔记",
    blocks: [{ type: "text", text: "填写需要保存的内容。" }],
    inputs: [{ id: "text", label: "内容", type: "text", required: true, maxLength: 2000 }],
    actions: [{ id: "save", label: "保存", style: "primary" }],
  },
});
```

处理入口随 Cordis scope 注销，应用重载需重新注册同名 handler。`create` 的 key 在 Session 内幂等，同 key 不能替换定义；`cancel(cardId)` 取消本应用待答卡片。默认一天过期，最多 30 天；定义最多 64 KiB，回复最多 32 KiB。模板为 approval/choice/form，输入支持文字、数字、布尔、单选、多选。

宿主先持久认领，再调用 handler；多设备只有首个有效回复执行。无效输入不会消耗卡片，取消、过期或 handler 销毁后拒绝提交。失败标为 failed，崩溃时处理中卡片标为 interrupted，均不自动重放。外部操作仍需应用以 cardId 保存业务检查点，不能宣称 exactly-once。

桌面和手机共享卡片状态；手机离线重试沿用同一命令 ID。卡片是 fn 产品交互记录，不替代 Agent Session 消息历史。原生文字块支持 Markdown，桌面卡片当前使用纯文字。
