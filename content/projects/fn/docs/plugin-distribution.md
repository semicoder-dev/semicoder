---
id: "docs-fn-plugin-distribution"
slug: "plugin-distribution"
title: "构建与分发"
description: "固定完整插件归档，验证后登记 schema 3 市场。"
date: "2026-10-31"
---

适用于 fn 0.6 / SDK 0.5。后端 ESM TypeScript 可由 Bun 运行；客户端必须先通过 SDK 构建为原版模块 factory。发行包包含源码入口、客户端产物、样式、图标和必要数据文件。

```sh
fn plugin check ./my-plugin
fn plugin build ./my-plugin
fn plugin pack ./my-plugin
```

## 发布顺序

1. 固定源码和依赖，完成检查、业务测试、独立构建及隔离宿主交互验收。
2. 将完整 npm 格式 tgz 发布到不可变的 HTTPS 地址，保存 SHA-256。
3. 从公开地址重新下载并核对摘要、元数据及实际安装。
4. 最后将通过验证的归档登记到 schema 3 市场。

索引包含 `schemaVersion: 3`、市场名称与 `plugins` 数组；每项提供 `packageName/name/description/version/url/sha256/peers` 及可选图标。`peers` 与包中声明一致；URL 不含凭证，版本已公开后不覆盖文件或移动标签。

官方例子见[marketplace.json](https://github.com/semicoder-dev/fn-plugins/blob/main/marketplace.json)。旧 schema 2 使用 applications/core，不能替代新格式。宿主会按真实归档和共享内核校验兼容性，不以市场显示可用代替安装验证。

插件包与应用安装包分别发行。安装或更新在后台空闲时生效；变更数据库格式前先定义插件自己的数据策略，不覆盖用户正在运行的目录。
