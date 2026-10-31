---
id: "docs-fn-plugin-manifest"
slug: "plugin-manifest"
title: "插件包配置"
description: "npm exports、精确 peers 和 fn 元数据。"
date: "2026-10-31"
---

适用于 SDK 0.5。包名建议为 `@semicoder/fn-<能力名>`。以下是后端加前端的基础结构；具体服务与 UI 依赖还须按实际使用补齐。

```json
{
  "name": "@semicoder/fn-example",
  "version": "0.1.0",
  "type": "module",
  "exports": { ".": "./src/index.ts", "./client": "./dist/client.js" },
  "files": ["src", "dist"],
  "peerDependencies": {
    "@deepseek-ai/cordis": "4.0.5",
    "@semicoder/fn-sdk": "0.5.0"
  },
  "fn": { "name": "示例", "clientSource": "./src/client.tsx" }
}
```

删除 `./client` 为纯后端，删除 `.` 为纯前端。`fn` 元数据允许 `name`、`description`、`icon`、`iconSvg`、`clientSource`、`bundle`、`profile`；不使用旧应用 `schemaVersion/sdkVersion` 或 `keepAlive` 声明。

`bundle/profile` 可指向包内 JSON 组合配置，沿用 Cordis Include 的插入/覆盖 patch 规则。包与实例分离，插件应明确是否支持多实例及命名空间配置。

## 依赖与图标

所有使用的共享模块声明精确 peer，私有依赖放在 dependencies。客户端不能打入第二份 React、Cordis、FIA 或 fn-ui。仅在 package.json 写相同版本号不足以证明构建输入一致，固定归档还须记录摘要。

图标使用包内静态单色 SVG，通过 `currentColor` 跟随主题。官方示例将图标放在 `src/icon.svg` 并复制到发行目录；归档和市场索引必须包含一致的元数据。
