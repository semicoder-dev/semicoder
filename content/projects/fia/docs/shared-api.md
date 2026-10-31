---
id: "docs-fia-shared-api"
slug: "shared-api"
title: "共享 API 与网关"
description: "统一类型契约与公共 HTTP 扩展。"
date: "2026-10-31"
---

defineAPI 的 methods/events 使用 Zod schema；implementAPI 提供唯一实现。CLI、页面和脚本共用验证与分发。方法上下文包括 native、app、emit、source、requestId、sessionId、signal。

http.fetch 提供 /api 下的业务路由，可返回标准 Response/SSE。http.pages 仅服务 /pages/ 下 GET/HEAD 的公开静态代码，不能暴露业务数据。http.authorize 可校验非浏览器后台凭证，不能绕过浏览器会话检查。

context.browserURL(path) 生成一次性浏览器链接；context.url(path) 取得真实网关地址。OAuth callbacks 仅接受显式 GET 路径，应用自行验证 PKCE 和一次性 state，不建立第二个回调端口。

原生 API 包括窗口、状态栏、受管理进程、文件对话框、剪贴板、通知、应用外观和 CloudKit。浏览器权限使用白名单，后端进程控制不对页面开放。
