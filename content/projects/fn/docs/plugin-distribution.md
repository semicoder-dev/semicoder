---
id: "docs-fn-plugin-distribution"
slug: "plugin-distribution"
title: "构建与分发"
description: "冻结依赖、归档、摘要与市场。"
date: "2026-10-31"
---

使用 `fn plugin check/build/pack .`。`.fn-build/` 是构建目录，`<id>-<version>.fn.tgz` 在应用根生成。归档包含后台 TypeScript、冻结运行依赖、构建后的页面、词条、初始摘要和资源。前端打包自身的 React、SDK 和 Fn UI。

应用使用精确的 SDK 0.7.0 与 Fn UI 0.2.0 npm 版本，开发工具必须在 workspace 外验证。安装不运行依赖脚本。市场 schema 1 的 applications 数组包含 id、version、name、description、url、sha256；远端使用 HTTPS。发布前独立下载并验摘要。

更新时目标应用先确认关闭，再切换不可变目录；页面与后台来自同一版本，其他应用继续运行。代码和数据分离，失败不改变有效安装。官方市场发布、npm、GitHub Release 和网站部署均单独执行。

官方 GitHub CI 对四个应用及行情示例执行检查、测试、打包和仓库外冻结消费；正式发布拒绝本地路径依赖及 overrides。标签必须与应用版本匹配，Release 下载校验通过后才切换市场，保留未更新条目。Fn 与 SDK/UI 发布复用完整质量门禁，公开 npm 归档还会匿名下载核对并独立消费。
