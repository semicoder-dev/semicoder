---
id: "docs-fn-plugin-distribution"
slug: "plugin-distribution"
title: "构建与分发"
description: "冻结依赖、归档、摘要与市场。"
date: "2026-10-31"
---

使用 `fn plugin check/build/pack .`。`.fn-build/` 是构建目录，`<id>-<version>.fn.tgz` 在应用根生成。归档包含后台 TypeScript、冻结运行依赖、构建后的页面、词条、小组件和资源。前端打包自身的 React、SDK 和 Fn UI。

应用使用精确的 SDK 0.6.0 与 Fn UI 0.1.0 npm 版本，开发工具必须在 workspace 外验证。安装不运行依赖脚本。市场 schema 1 的 applications 数组包含 id、version、name、description、url、sha256；远端使用 HTTPS。发布前独立下载并验摘要。

更新时目标应用先确认关闭，再切换不可变目录；页面与后台来自同一版本，其他应用继续运行。代码和数据分离，失败不改变有效安装。官方市场发布、npm、GitHub Release 和网站部署均单独执行。
