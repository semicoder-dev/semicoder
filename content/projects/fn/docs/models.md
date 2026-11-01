---
id: "docs-fn-models"
slug: "models"
title: "模型与账号"
description: "ChatGPT 订阅授权、可用模型与 Responses。"
date: "2026-11-01"
---

点击底部 Dock 的「设置」，在模型页添加 ChatGPT 账号，通过官方授权页面登录。Fn 验证身份和权限，宿主持有凭证，应用只取得连接 ID。可查看账号可用模型，选择默认模型，重新连接或退出登录。

首期只支持官方 ChatGPT 账号订阅。模型目录以账号返回结果为准；额度限制明确显示错误。退出登录取消该账号在途请求，撤销凭证；网络导致远端撤销未确认时明确提示。

独立「模型目录」应用 0.2.1 已在官方市场提供，使用 [models.dev JSON API](https://models.dev/catalog.json?type=all) 查询模型规格、能力与供应商报价，支持搜索、筛选和缓存。在「应用」中心刷新官方市场并安装，无需更新 Fn 或配置 API Key。目录仅供查阅，不会为 Fn 添加模型连接，也不改变上述订阅调用范围。上游目录与实际账号可用模型分别维护，报价以供应商为准。

支持名称/ID 搜索，按机构、模型类型、输入模态和能力筛选，排序分页后查看规格及供应方案。顶部搜索、筛选与表头保持可见，底部分页固定，中间表格独立滚动；换页或重新筛选后回到表格顶部。窄窗可横向滚动表格，详情内容独立滚动，返回栏保持可见。

价格以美元/百万 token 展示，`—` 表示未提供；阶梯与额外费用可在供应商详情中查看上游字段。一小时按需刷新本机缓存，也可手动刷新；网络失败保留上次有效数据并提示过期。关闭应用停止后台，重新打开保留缓存。

[模型目录 0.2.1 发行说明](https://github.com/semicoder-dev/fn-plugins/releases/tag/models-dev-v0.2.1)

应用调用 OpenResponses 请求与事件，连接 ID 和取消信号位于 Fn 调用上下文。`store:false`、`stream:true`、完整 input 数组为调用要求。不支持参数报错，收到完成终态才表示成功；中断和未完成不能伪装成完整结果。

宿主管理传输和并发，应用负责上下文、工具执行与 Agent loop。参见[官方授权](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)、[官方模型调用](https://developers.openai.com/siwc/token-sharing-open-source/models-and-inference)与[OpenResponses](https://www.openresponses.org/specification)。
