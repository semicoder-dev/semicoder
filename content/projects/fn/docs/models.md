---
id: "docs-fn-models"
slug: "models"
title: "模型与账号"
description: "ChatGPT 订阅授权、可用模型与 Responses。"
date: "2026-10-31"
---

点击底部 Dock 的「设置」，在模型页添加 ChatGPT 账号，通过官方授权页面登录。Fn 验证身份和权限，宿主持有凭证，应用只取得连接 ID。可查看账号可用模型，选择默认模型，重新连接或退出登录。

首期只支持官方 ChatGPT 账号订阅。模型目录以账号返回结果为准；额度限制明确显示错误。退出登录取消该账号在途请求，撤销凭证；网络导致远端撤销未确认时明确提示。

应用调用 OpenResponses 请求与事件，连接 ID 和取消信号位于 Fn 调用上下文。`store:false`、`stream:true`、完整 input 数组为调用要求。不支持参数报错，收到完成终态才表示成功；中断和未完成不能伪装成完整结果。

宿主管理传输和并发，应用负责上下文、工具执行与 Agent loop。参见[官方授权](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)、[官方模型调用](https://developers.openai.com/siwc/token-sharing-open-source/models-and-inference)与[OpenResponses](https://www.openresponses.org/specification)。
