---
id: "docs-fia-development"
slug: "development"
title: "应用开发"
description: "共享 API、Bun 构建与原生生命周期。"
date: "2026-11-03"
---

fia.config.ts 定义应用身份、backend、web、agent、权限、签名和更新。shared/api.ts 使用 Zod 定义共享方法和事件；后台 implementAPI 注册，页面 createClient 调用。

FIA 提供统一本机网关，HTTP JSON、SSE、WebSocket 共用 Bun 服务。浏览器通过一次性授权入口连接真实后端；默认原生标题栏提供浏览器入口。开发修改重建并刷新页面，后台变更受控重启，不保留组件状态。

开发网关到本机后端的通信自动直连，不经过 shell 的 HTTP 代理，不要求手工设置 `NO_PROXY`；应用外网请求仍沿用代理配置。

Fn 0.8.5/build 21 对应的共仓框架源码 `7736dce` 已修复 Bun 1.4.4 下本机开发服务 readiness 探测受代理影响的问题。readiness 与网关转发是不同链路，不能把先前的网关直连当成探测已验证；该修复属于框架 TypeScript，不改业务 API 或外网代理。独立 npm 仍为 0.18.0，未包含此开发修复。

窗口可以声明 managedClose，用户关闭发出 windows.closeRequested，由应用确认后调用 windows.close。statusItem.setMenu 设置动态菜单及 locale。processes.spawn/state/stop 管理独立进程组，后端重启与退出回收全部受管进程。

框架源码先完成 prepare/check/pack/verify，再由应用消费同一构建产物。Fn 管理应用平台业务，FIA 不读取应用 manifest 或实现模型逻辑。
