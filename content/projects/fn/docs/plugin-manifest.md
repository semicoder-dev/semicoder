---
id: "docs-fn-plugin-manifest"
slug: "plugin-manifest"
title: "插件结构与生命周期"
description: "普通 npm 包、Cordis 入口、精确共享依赖和应用图标。"
date: "2026-10-21"
---

适用于 fn 0.5.0 / SDK 0.4.0。插件沿用 npm 字段；`fn` 元数据只描述应用身份和展示，不再使用旧 `frontend/backend/sdkVersion` 清单。

## 包结构

```json
{
  "name": "@example/my-app",
  "version": "0.1.0",
  "type": "module",
  "files": ["src", "dist"],
  "exports": { ".": "./src/index.ts", "./client": "./dist/client.js" },
  "fn": {
    "schemaVersion": 1,
    "id": "my-app",
    "name": "我的应用",
    "description": "用途说明",
    "icon": "记",
    "iconSvg": "dist/icon.svg"
  },
  "peerDependencies": {
    "@deepseek-ai/cordis": "4.0.5",
    "@semicoder/fn-sdk": "0.4.0",
    "react": "19.2.7",
    "react-dom": "19.2.7"
  }
}
```

`exports["."]` 是标准 Cordis 插件，支持 `apply`、`inject`、配置与 effect；Bun 可直接运行 ESM TypeScript。`./client` 可选，无页面服务无需提供。后端源码及所有相对导入必须在包内，浏览器入口必须预构建。

## 身份与图标

`fn.id` 使用小写字母开头的小写字母、数字和连字符，最长 100 字符；它决定应用数据归属，发布后不要随意改变。`name` 最长 100 字符，`description` 最长 1000 字符。`icon` 可为 1–4 字符。

构建器从 `src/icon.svg` 生成 `dist/icon.svg`，上限 8 KiB。宿主以图片或 CSS mask 显示，文字作为回退；不把 SVG 注入 DOM。服务应用也可携带图标。

## 共享依赖

精确声明实际使用的共享核心 peer dependencies。Agent、Session、工具类型直接从锁定的 `@deepseek-ai/*` 包导入；dsh 家族为 0.2.1-rc.2。运行目录会检查依赖解析，不能携带另一份核心服务。

页面生命周期配置由 `clientApp.page({keepAlive:true,...})` 贡献，不放入旧清单字段。应用是可信本地代码，元数据不构成权限沙箱。
