---
id: "docs-fn-plugin-manifest"
slug: "plugin-manifest"
title: "应用定义"
description: "fn.app.json、能力声明与入口。"
date: "2026-10-31"
---

`fn.app.json` 描述应用身份和宿主 API 契约，包名与应用 ID 使用小写。manifestVersion 与 hostApi 均为 1；应用版本使用 x.y.z。

| 字段                | 含义                                            |
| ------------------- | ----------------------------------------------- |
| id / version / name | 身份、版本和名称，名称可引用词条                |
| description / icon  | 可选说明与包内 SVG/PNG/JPEG/WebP                |
| entries             | 可选 backend TypeScript 和 page TSX 入口        |
| locales             | 默认 en 或 zh-CN，messages 指向包内 JSON        |
| uses                | models.responses、widgets.publish、chat.receive |
| contributes.widgets | 小组件 ID、固定网格 size、初始 JSON，最多 4 个  |
| contributes.chat    | 可选聊天 ID 与名称，需要后台和 chat.receive     |

入口均可选。没有页面时使用宿主状态页；无后台时不启动子进程。每个已安装应用一个普通实例，开发实例单独隔离。完整示例见[应用开发](/fn/docs/plugin-development)。

小组件的 `size` 声明唯一的列数 x 行数，例如 `"2x2"`，每个维度为 1–4。尺寸由插件决定，宿主按固定网格排列；改变尺寸需更新应用包。参见[小组件](/fn/docs/widgets)。
