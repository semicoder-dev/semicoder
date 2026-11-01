---
id: "docs-fn-plugin-manifest"
slug: "plugin-manifest"
title: "应用定义"
description: "fn.app.json、能力声明与入口。"
date: "2026-10-31"
---

`fn.app.json` 描述应用身份和宿主 API 契约，包名与应用 ID 使用小写。manifestVersion 为 1，hostApi 为 2；应用版本使用 x.y.z。

| 字段                | 含义                                                |
| ------------------- | --------------------------------------------------- |
| id / version / name | 身份、版本和名称，名称可引用词条                    |
| description / icon  | 可选说明与包内 SVG/PNG/JPEG/WebP                    |
| entries             | 可选 backend TypeScript 和 page TSX 入口            |
| locales             | 默认 en 或 zh-CN，messages 指向包内 JSON            |
| uses                | models.responses、application.summary、chat.receive |
| contributes.summary | 可选唯一摘要，initial 指向可选的包内初始 JSON       |
| contributes.chat    | 可选聊天 ID 与名称，需要后台和 chat.receive         |

入口均可选。没有页面时使用宿主状态页；无后台时不启动子进程。每个已安装应用一个普通实例，开发实例单独隔离。完整示例见[应用开发](/fn/docs/plugin-development)。

动态摘要必须声明 `application.summary` 能力和 backend 入口；静态初始摘要不需要后台。摘要不接受尺寸或任意布局，构建和安装均校验初始文件。参见[应用摘要](/fn/docs/application-summaries)。
