---
id: "docs-fn-automation"
slug: "automation"
title: "CLI 与自动化"
description: "通过动态命名空间 API 和插件构建命令操作 fn。"
date: "2026-10-31"
---

适用于 fn 0.6 / SDK 0.5。应用菜单可安装正式 `fn` CLI；开发和预览分别使用 `fn-dev`、`fn-preview`。CLI 连接现有实例，不启动额外业务服务器。

## 查询运行状态

```sh
fn call plugins.status --json '{}'
fn call plugins.list --json '{}'
fn call market.list --json '{}'
fn call connections.list --json '{"offset":0,"limit":50}'
fn open --browser
```

可调用方法由当前启用的插件动态注册。以运行实例的命令帮助及对应插件 contract 为准；分页列表须处理返回的游标。旧 `apps.invoke` 聚合入口不适用于本版。

## 构建插件

```sh
fn plugin check ./my-plugin
fn plugin build ./my-plugin
fn plugin pack ./my-plugin
```

源码仓库可用 `bun run agent plugin ...` 执行同一组命令。SDK 0.5 提供检查、构建与打包，不提供旧 `application create` 模板命令。

浏览器授权属于当前实例，后台重启后重新连接。页面生命周期与后台任务分开，取消任务必须通过所属服务的显式契约；不要直接写运行目录或数据库。
