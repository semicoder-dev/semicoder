---
id: "docs-fia-distribution"
slug: "distribution"
title: "构建与分发"
description: "固定运行时、代码产物、签名与验收。"
date: "2026-10-31"
---

fia build 生成应用，build --dmg 生成并验收本地镜像。fia release 使用 Developer ID 签名、公证并检查 Gatekeeper；正式发布需要明确授权与 Apple 配置。

固定运行时包含 Swift Host、CLI 客户端与 Bun 1.4.4；代码产物包含后台、页面和资源。用户数据写 context.app.dataDirectory。原生能力、权限或内置运行时变化需要完整安装包。

CloudKit 按构建模式指定 entitlements、profile 和容器白名单。模拟器和普通 ad-hoc 包不能代替签名真机验收。框架归档必须完成独立安装、应用构建和运行检查，报告使用摘要记录实际内容。
