---
id: "docs-fn-plugin-distribution"
slug: "plugin-distribution"
title: "构建与分发"
description: "不可变应用归档、Cordis 市场与等待空闲的后台更新。"
date: "2026-10-21"
---

适用于 fn 0.5.0 / SDK 0.4.0。后端源码可由 Bun 直接执行，浏览器入口必须预构建。

## 构建和验包

```sh
bun install --frozen-lockfile --ignore-scripts
fn application check
fn application build
fn application pack
```

也可使用 `@semicoder/fn-sdk/build-cordis` 的 `runCordisTool`、`checkCordisApplication`、`buildCordisApplication` 和 `packCordisApplication`。构建失败保留前一次完整 dist；pack 遵循 npm files/exports 并检查实际入口。发布包不能依赖相邻源码或未打包文件。

## 发布来源

手动安装支持 npm、HTTPS tgz、本地归档、`github:owner/repo#ref`、`git+https://…#ref` 以及 HTTPS `.git#ref`。Git 只归档指定 ref 的已提交内容，不运行构建脚本，需提交前端产物；使用固定 SHA 才能复现。

官方应用通过 GitHub Releases 分发，市场索引为 `marketplace-cordis.json`，schemaVersion 为 2。条目包含 id、packageName、name、description、version、HTTPS url、sha256 及精确 core 依赖版本，可带 icon/iconSvg。新索引不混入旧 manifest 插件。

每个版本使用不可变 URL 和 SHA-256。先公开并独立验收归档，再通过 PR 更新市场；不能用相同版本字符串代替产物一致性验证。

## 安装与更新

宿主先解析完整依赖图，生成锁文件并检查核心单例，再启动检查候选运行目录。等待任务空闲后停止接收新任务、flush、销毁并重启 Bun；FIA 保留窗口，前端重连。失败保留可用目录。

运行阶段关闭自动安装。卸载或停用释放应用作用域；数据保留。目录回滚与数据库回滚分开处理，新格式写入后不能直接启动旧内核。应用更新与插件更新独立，原生宿主变化需要完整 DMG。
