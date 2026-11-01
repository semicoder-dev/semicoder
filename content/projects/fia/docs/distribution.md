---
id: "docs-fia-distribution"
slug: "distribution"
title: "构建与分发"
description: "固定运行时、代码产物、签名与验收。"
date: "2026-11-01"
---

fia build 生成应用，build --dmg 生成并验收本地镜像。fia release 使用 Developer ID 签名、公证并检查 Gatekeeper；正式发布需要明确授权与 Apple 配置。

固定运行时包含 Swift Host、CLI 客户端与 Bun 1.4.4；代码产物包含后台、页面和资源。用户数据写 context.app.dataDirectory。原生能力、权限或内置运行时变化需要完整安装包。

CloudKit 按构建模式指定 entitlements、profile 和容器白名单。模拟器和普通 ad-hoc 包不能代替签名真机验收。框架归档必须完成独立安装、应用构建和运行检查，报告使用摘要记录实际内容。

## 固定运行时与候选产物

Fn 0.8.2 使用共仓 FIA 的固定公开运行时构建；该接口尚未包含在独立公开 npm 0.18.0 中。`fia release --runtime-base <锁文件> --with-update` 先验证公开基线安装包的摘要、签名、公证和更新清单，复用原生 Host、Bun、固定 CLI/preload 与资源；只构建一次代码，生成完整 DMG 和内容相同的签名更新包。

版本/build 和普通代码变化不改变兼容身份；权限、原生输入或固定资源变化必须建立新基线，验证失败不能自动回退重编。新安装包重新签名后的完整性摘要与运行时兼容身份分别校验。

应用发布方应对这份候选执行实际旧版在线更新、重启和失败回退验收，再发布同一份产物。先上传不可变文件并验证，再公开 Release，最后切换更新清单。新包启动和完整 DMG 替换验证不能代替在线更新；代码回退不回滚业务数据。
