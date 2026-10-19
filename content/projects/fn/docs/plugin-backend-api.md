---
id: "docs-fn-plugin-backend-api"
slug: "plugin-backend-api"
title: "后端 API"
description: "定义带 schema 的插件方法，使用后端模型和 KV 上下文。"
date: "2026-10-16"
---

> 适用于 fn 0.4.5 / SDK 0.3.5，manifest 为 0.3。先按[安装指南](/fn/docs/installation)安装 fn；旧名称插件和旧应用数据不迁移。

本页适用于 **fn 0.4.5 / SDK 0.3.5**。后端在宿主 Bun 进程运行，适合业务校验、数据整理和调用宿主模型。纯前端笔记插件不需要后端。

## 定义与调用

| 导出         | 签名与行为                                                                                                                              |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| definePlugin | `(plugin: BackendPlugin): BackendPlugin`；返回传入对象，默认导出给宿主加载                                                              |
| defineMethod | `(description: string, schema: z.ZodType<T>, handler: (input: T, context: PluginContext) => unknown 或 Promise<unknown>): PluginMethod` |
| z            | SDK 使用的 Zod；输入 schema 同时用于 JSON Schema 发现与调用时 parse                                                                     |

`BackendPlugin.methods` 为必填的方法名到 `PluginMethod` 的映射；可选 `activate(context)`、`dispose()` 返回 void 或 `Promise<void>`。`PluginMethod` 包含 `description`、JSON 格式 `inputSchema` 和 `execute(input, context)`，优先用 defineMethod 生成。

schema 必须能转换成 JSON Schema；避免 Date、自定义 transform 等不能表示的输入。调用必须传 JSON，方法返回值也必须是 JSON，不能返回 undefined、函数或循环对象。返回 JSON 序列化后最多 **256 KiB**；校验或处理器抛错会使前端 Promise reject。

## PluginContext

| 成员                  | 返回值              | 说明                                           |
| --------------------- | ------------------- | ---------------------------------------------- |
| pluginId              | readonly string     | 当前插件 ID，无需由调用方传入                  |
| models.list()         | ModelInfo[]         | **同步**读取宿主模型目录                       |
| models.start(request) | `Promise<ModelRun>` | 启动宿主模型任务，返回运行记录，不等待生成结束 |
| kv.get(key)           | JSONValue 或 null   | **同步**读取；不存在返回 null                  |
| kv.set(key, value)    | `Promise<void>`     | 每值 JSON UTF-8 最多 64 KiB，等待持久化        |
| kv.delete(key)        | `Promise<void>`     | 删除并持久化                                   |

上下文没有 runs、invoke 或事件订阅接口。模型请求及限制见[前端 API](/fn/docs/plugin-client-api#请求与返回类型)。前端 KV 所有操作均为 Promise，set/delete 返回 null；不要把前端与后端签名混用。

## 完整示例：保存并统计笔记

在入门的 `my-notes` 项目添加 `src/backend.ts`，并在 `package.json.fn` 添加 `"backend": "dist/backend.js"`。源码文件与清单必须同时存在。

```ts
import { defineMethod, definePlugin, z } from "@semicoder/fn-sdk/runtime";

export default definePlugin({
  methods: {
    saveNote: defineMethod(
      "保存笔记并返回字符数",
      z.strictObject({ text: z.string().max(8000) }),
      async ({ text }, context) => {
        await context.kv.set("note", text);
        return { text, characters: Array.from(text).length };
      },
    ),
  },
});
```

用下列代码替换 `src/client.tsx`。`invoke` 的方法名是本插件 methods 的键，泛型只提供 TypeScript 提示，不会验证返回数据。

```tsx
import "./styles.css";
import { useEffect, useState } from "react";
import { createPluginClient } from "@semicoder/fn-sdk/client";
import { Alert, Button, Field, Page, PageHeader, Textarea } from "@semicoder/fn-sdk/ui";

const client = createPluginClient("my-notes");
type SavedNote = { text: string; characters: number };

export default function BackendNotes() {
  const [text, setText] = useState("");
  const [ready, setReady] = useState(false);
  const [busy, setBusy] = useState(false);
  const [message, setMessage] = useState("");
  const [error, setError] = useState("");
  useEffect(() => {
    let live = true;
    void client.kv
      .get("note")
      .then((value) => {
        if (live) {
          setText(typeof value === "string" ? value : "");
          setReady(true);
        }
      })
      .catch((reason: unknown) => {
        if (live) setError(String(reason));
      });
    return () => {
      live = false;
    };
  }, []);
  async function save() {
    setBusy(true);
    setError("");
    setMessage("");
    try {
      const result = await client.invoke<SavedNote>("saveNote", { text });
      setMessage(`已保存 ${result.characters} 个字符`);
    } catch (reason) {
      setError(String(reason));
    } finally {
      setBusy(false);
    }
  }
  return (
    <Page className="p:space-y-4">
      <PageHeader title="带后端的笔记" />
      <Field label="笔记">
        <Textarea
          value={text}
          maxLength={8000}
          disabled={!ready || busy}
          onChange={(event) => setText(event.target.value)}
        />
      </Field>
      <Button disabled={!ready} loading={busy} onClick={() => void save()}>
        保存并统计
      </Button>
      {message && <Alert tone="success">{message}</Alert>}
      {error && <Alert>{error}</Alert>}
    </Page>
  );
}
```

继续执行 `bun run check`、`bun run build`、`bun run pack` 并安装归档。修改已安装插件时提高包版本后更新；仅修改源码不会改变宿主正在运行的版本。

## 初始化与释放

`activate(context)` 用于短时初始化；完成前插件尚未 active，不要在这里调用 `context.models.start`。激活失败会让插件进入 error 状态。将模型启动放在用户触发的方法里，返回 ModelRun 后由前端订阅进度。

`dispose()` 应释放插件自己创建的资源；不要依赖它在崩溃时保存数据。没有宿主管理的自建持久任务接口，避免创建无法取消的常驻后台任务。方法调用期间会占用插件调用计数，阻止冲突的停用和更新，处理器应及时结束。

## 故障定位

| 症状               | 检查项                                           |
| ------------------ | ------------------------------------------------ |
| 插件方法不存在     | methods 的键、前端 invoke 名称、是否安装了新归档 |
| 插件未就绪或已停用 | 应用中心的 active 状态、激活错误、是否正在更新   |
| 输入校验失败       | JSON 形状、strictObject 多余字段、长度约束       |
| 返回结果失败       | 是否为 JSON、是否超过 256 KiB、是否遗漏 return   |
| 激活失败           | backend 默认导出、清单入口、模块初始化异常       |

类型来源：runtime.ts：`packages/sdk/src/runtime.ts`（[v0.4.5 源码](https://github.com/semicoder-dev/fn/tree/v0.4.5)）、types.ts：`packages/sdk/src/types.ts`（[v0.4.5 源码](https://github.com/semicoder-dev/fn/tree/v0.4.5)）。宿主最终执行校验见插件实现：`backend/plugins.ts`（[v0.4.5 源码](https://github.com/semicoder-dev/fn/tree/v0.4.5)）。

## 后端网络工具

fn 0.4.5 / SDK 0.3.5 起，在 `PluginContext.web` 提供 `search(input, { signal? })` 和 `fetch(input, { signal? })`，直接等待结果；参数、错误和限制与[前端网络工具](/fn/docs/plugin-client-api#网络工具)一致。

```ts
import { defineMethod, definePlugin, z } from "@semicoder/fn-sdk/runtime";
export default definePlugin({
  methods: {
    readPage: defineMethod("读取网页", z.object({ url: z.url() }), (input, context) =>
      context.web.fetch({ url: input.url, maxBytes: 50_000 }),
    ),
  },
});
```

agent-webtool 由宿主后端统一提供，插件无需安装或将它打进前端。每次调用创建独立来源上下文，不共享编号、也不提供会话恢复。普通 plugins.invoke 仍受原有调用时间限制；可能较慢的操作优先使用前端 client.web 或后端自行管理的异步流程，不把 Promise 包装当作无限等待的 RPC。

## 机器人聊天（开发分支，尚未发布）

以下能力不包含在公开 fn 0.4.5 / SDK 0.3.5 中，只能使用配套开发源码构建。未发布的旧 `communication` / `remoteActions` 接口已被直接替换，不提供兼容或数据迁移。

`@semicoder/fn-sdk/chat` 提供类型与校验契约，后端可选 `context.chat` 提供 `sendText`、`request`、`get`、分页 `list` 和 `cancel`。宿主绑定当前插件及其固定会话，查询和取消仅限自身消息。发送立即返回本地持久 ID，云端确认和宿主处理状态分别显示。

插件显式声明 `BackendPlugin.chat` 的 `version`、`onMessage` 和 `onResponse`。手机与独立 Mac fn Chat 都是纯聊天端，只能主动发送文字；宿主不使用 AI 自动解释文字，也不公开普通 `methods`。同一 iCloud 账号下，每台 fn 宿主作为可切换服务端，一个宿主与一个插件对应固定机器人聊天。其他服务器仍后台同步和提醒。

机器人可发完整文字或请求卡片，多次发送产生多条消息。卡片是有限版本化 fn JSON：标题、摘要、文字/字段/分隔块、按钮，以及文本、数字、布尔、单选和多选输入；支持批准、选择和表单模板，原生渲染，不执行 HTML 或脚本。未知卡片显示摘要且不可提交。请求内容不可变，进度和处理状态独立更新。

2026-10-19 本地开发客户端将 `sendText` 正文和卡片 `text` 块渲染为 Markdown，包含列表、引用、代码块、表格和链接；插件继续传原始字符串，协议无需变化。标题、摘要、字段与按钮仍显示纯文字。代码与宽表格可横向滚动，支持复制代码和原文；链接仅允许 HTTP(S) 与 mailto，图片不自动加载，HTML 不执行。iOS 0.2.1（3）已完成 Apple 处理并加入既有本人内部 TestFlight 组，当前分发状态见[安装说明](/fn/docs/installation)；旧 0.2.0（2）不含此更新。

`request` 不等待用户回复；插件保存请求 ID 和业务检查点，后续持久 `onResponse` 继续处理。回调带稳定事件 ID、会话 ID、取消信号及进度接口，同会话串行、默认十分钟超时。处理中断不自动重放，须核对实际业务结果。请求默认二十四小时；回复、取消、过期通过同 zone 条件写入竞争，首个有效回复生效。执行前重验目标宿主、插件启用状态、版本、有效期与全部输入。

消息 payload 上限 64 KiB，无附件、逐字流式输出或历史删除；聊天端共享已读、归档与卡片结果。可见提醒仅针对新机器人消息，其余变更只触发同步。系统账号切换严格隔离旧队列与缓存；离线文字保留原目标，不随服务端切换重新路由。

新客户端工程位于 fn 源码 `clients/apple/`，验收插件在 `chat/acceptance/`。完整四设备和系统通知矩阵仍需真机验收。iOS 0.2.1（3）已进入本人内部 TestFlight，生产 schema/订阅已部署，用户于 2026-10-19 确认 TestFlight 安装恢复正常，生产手机业务/通知仍待验收；这不代表公开 SDK 或 Mac 宿主已支持聊天。当前范围和限制见[安装说明](/fn/docs/installation)。

## Agent loop 与自定义工具（开发分支，未发布）

开发宿主新增 `PluginContext.agents`，与前端 agents 方法一致，可创建会话、发送消息、读取状态/持久事件和取消任务。公开 fn 0.4.5 / SDK 0.3.5 不包含此能力；使用同一次开发构建的宿主和 SDK 快照。

工具通过 `BackendPlugin.tools` 声明，`defineTool` 从 runtime 导出。模型只看到会话显式选择的工具，普通 methods 不会自动暴露，也不会调用其他插件的私有工具。

```ts
import { definePlugin, defineTool, z } from "@semicoder/fn-sdk/runtime";

export default definePlugin({
  methods: {},
  tools: {
    remember: defineTool(
      "保存调研结论",
      z.object({ text: z.string().max(2000) }),
      async (input, ctx) => {
        ctx.signal.throwIfAborted();
        await ctx.kv.set("conclusion", input.text);
        return {
          content: [{ type: "text", text: "已保存" }],
          meta: { preview: input.text },
        };
      },
    ),
  },
});
```

工具参数必须为 JSON Schema object。宿主在执行前校验模型生成的 JSON；content 交给模型，meta 仅供展示及历史重建。工具上下文新增 signal、sessionId、callId，仍可使用模型、网络、KV 和显式聊天能力。工具按顺序执行，错误回填为 isError 结果，不自动重试。

工具必须传递并响应 AbortSignal；取消不回滚已发生的副作用。整个 Agent turn 阻止插件停用/更新/卸载、对应模型编辑/移除及应用更新；重启中断不重放。后端 onChange 订阅随插件 dispose 清理。Agent 会话日志与 fn Chat 跨设备聊天是独立协议，插件自行决定何时将结果回复到聊天端。
