---
id: "docs-fn-plugin-development"
slug: "plugin-development"
title: "开发第一个插件"
description: "使用 Cordis、Bun TypeScript 和共享 React 创建 fn 0.5 应用。"
date: "2026-10-21"
---

> [全插件化候选](/fn/docs/plugins-preview)是未发布的独立开发方案；本页继续描述当前公开版本。

适用于 fn 0.5.0 / SDK 0.4.0。插件是由 Bun 执行的 Cordis npm 包，可提供独立 React 页面或仅提供后台服务。旧 `plugin` 命令和旧插件接口不适用于此版本。

## 创建应用

安装桌面版并安装其命令行后，使用 Bun 1.4.3：

```sh
fn application create ./my-app --name 我的应用
cd my-app
bun install --ignore-scripts
fn application check
fn application build
fn application pack
```

开发宿主使用 `fn-dev`，预览使用 `fn-preview`。源码工作区可运行 `bun run agent application …`。`create --service` 生成无页面服务包；目标目录必须不存在。CLI 复制宿主固定 SDK 归档到 `vendor/fn-sdk.tgz`，独立项目无需读取宿主源码。

## 后端

```ts
import type { Context } from "@deepseek-ai/cordis";
import type {} from "@semicoder/fn-sdk";
export const inject = ["app"];
export function apply(ctx: Context) {
  ctx.app.method("echo", (input) => input);
  ctx.app.chat({ id: "main", name: "我的助手", systemPrompt: "帮助用户完成任务。" });
}
```

`app` 绑定当前应用作用域。业务方法负责检查 JSON 输入，资源通过 Cordis effect 清理。应用拥有后台会话，页面关闭或手机断线不取消任务。

## 页面

```tsx
import type { Context } from "@deepseek-ai/cordis";
import type {} from "@semicoder/fn-sdk/cordis-client";
import { Button } from "@semicoder/fn-sdk/ui";
export const inject = ["clientApp"];
export function apply(ctx: Context) {
  const api = ctx.clientApp;
  api.page({
    id: "main",
    title: "我的应用",
    component: () => <Button onClick={() => void api.kv.set("hello", "world")}>保存</Button>,
  });
}
```

使用构建器生成 `dist/client.js`，不要内嵌第二份 React、Cordis 或 SDK UI。将 `.tgz` 安装到应用中心后，宿主准备完整依赖目录，等待任务空闲再重启后台加载。开发修改同样需要重新构建/打包和更新安装。

后续阅读：[包结构](/fn/docs/plugin-manifest)、[后端 API](/fn/docs/plugin-backend-api)、[前端 API](/fn/docs/plugin-client-api)。
