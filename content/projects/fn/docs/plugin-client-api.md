---
id: "docs-fn-plugin-client-api"
slug: "plugin-client-api"
title: "前端 API"
description: "页面作用域、分页、KV、模型连接和持久 Session 卡片。"
date: "2026-10-21"
---

适用于 fn 0.5.0 / SDK 0.4.0。前端通过 Cordis 注入 `clientApp`，不再使用 `createPluginClient`。

## 注册页面

从 `@semicoder/fn-sdk/cordis-client` 导入类型扩展，在 `apply(ctx)` 中调用 `ctx.clientApp.page({id,title,component})`。返回注销函数，并随 Cordis 作用域销毁。可设置 `keepAlive:true` 保留当前窗口切页状态；后台换代或窗口关闭后重新挂载，持久内容应写入 KV。

## 业务方法与 KV

| 接口                      | 用途                               |
| ------------------------- | ---------------------------------- |
| `api.context.appId`       | 当前应用 ID                        |
| `api.invoke(name,args)`   | 调用本应用后端方法，JSON 输入输出  |
| `api.kv.get(key)`         | 异步读取 JSON，不存在返回 null     |
| `api.kv.set(key,value)`   | 保存 JSON，单值上限 256 KiB UTF-8  |
| `api.kv.delete(key)`      | 删除本应用键                       |
| `api.kv.list()`           | 自动翻页，返回完整键值列表         |
| `api.call(method,params)` | 调用宿主会话等 RPC                 |
| `api.list(method,params)` | 自动读取分页 RPC，失败拒绝整次读取 |

## 模型和会话

`api.list("models.saved")` 获取用户已添加模型；每项包含 `connectionId`、`id` 和名称。按连接筛选后构造 `{connectionId,modelId:item.id}`，由后端创建 Session。`connections.models` 是完整账号目录，不能将预设目录当成账号权限证明。

`api.list("sessions.list", {appId:api.context.appId})` 读取应用会话；`sessions.projection` 提供产品展示，Agent 原始 replay 不直接成为页面协议。关闭页面不停止会话；取消使用显式命令。

直接读取分页 RPC 时处理 `{items,nextOffset,hasMore}`，每页最多 100 项和 512 KiB；不能假定一页包含全部内容。React effect 必须清理订阅和定时器。

## 卡片

`cards.list({sessionId,offset,limit})` 读取持久卡片；`cards.respond({sessionId,cardId,actionId,values})` 回复。共享类型和验证来自 `@semicoder/fn-sdk/cards`。提交后按持久状态展示完成/失败/中断，不能把网络返回当作外部副作用完成。详见[后端卡片](/fn/docs/plugin-backend-api#session-自定义卡片)。
