# Fn 0.8.5 官网同步与 QA

## 当前状态与授权边界

- 日期：2026-11-03。本站基线 `f09396f`，本地分支 `codex/fn-085-release`；开始时工作树干净，目标分支不存在后创建，未覆盖分支。
- 准备阶段保留公开 0.8.4 默认入口；在源项目确认 0.8.5 正式公开产物、旧版完整安装和公网门禁通过后，按追加指令切换本地默认入口与正式正文，不再保留本版候选预告。
- Fn 0.8.5/build 21 已公开，源项目固定 `7736dce5a45e5bd94e169fb63c6b3918e15d806a`。签名候选 [41488180080](https://github.com/semicoder-dev/fn/actions/runs/41488180080) attempt 3 / artifact `13192008893` 的 13 项门禁通过，正式流程 [41553571685](https://github.com/semicoder-dev/fn/actions/runs/41553571685) 提升同一产物，真实公网报告 9 项通过，精确 target build 21、`mode=public`、`originalAppsVerified=true`。
- 子代理准备阶段不提交、合并、推送、部署、审批生产或修改远端保护；下方“部署与线上验收结果”由父任务按本次明确授权完成。官网源码同步、本地 QA、两环境部署与独立线上核验分别记录。
- SDK 0.8.0 / UI 0.2.2、翻译 0.3.1、其余独立应用、手机及 FIA npm 0.18.0 不改版。独立应用无需因本次宿主修复重装。

## 正式产物与基线证据

- 默认 DMG：[Fn-0.8.5-21-mac-arm64.dmg](https://github.com/semicoder-dev/fn/releases/download/v0.8.5/Fn-0.8.5-21-mac-arm64.dmg)，45,846,716 字节，SHA-256 `8f568cee79f078918c24c2bb20e621888a9240239e8806ce3f98daa5901fda85`；[发行说明](https://github.com/semicoder-dev/fn/releases/tag/v0.8.5)。签名、公证和公开字节由源项目发行门禁核对，本站不以 CI 候选或猜测地址代替。
- 源项目工作流外匿名检查了 11 个 Release 附件、12 个 Pages 更新文件与 live latest，共 24 项；独立 SHA-256/Ed25519 和逐字节核对通过，宿主官方 verify-public release/active 也通过。
- 本版真实 Swift Host 菜单变化要求 0.8.4 及更早版本完整 DMG 替换；现有应用、模型连接、业务数据、摘要与偏好保留。完整安装证据与旧版本在线代码更新证据不可互相替代，不要求删除数据或重装独立应用。
- 从正式公开包以生产签名配置 capture，新 runtimeId `8287e13203e92ef6f6aa6f2bdb3b41f1ddba0fa2357d7b5e20eeff88911afb1a`；来源提交、签名、公证、Gatekeeper、固定 JS 与权限核对通过。新锁 SHA-256 `7a0ad3deba97ed0e8d203744ec6b0145fed2264acb6572750bb57a431bab2891`，原生输入检查和 project/temporary 两份独立空缓存代码复用通过。
- 新基线与默认 code 恢复已随源项目 `3b2b580a9daf261fd977edf04e637ad6f2f6e9f1` 快进并推送 main，[CI 41554530579](https://github.com/semicoder-dev/fn/actions/runs/41554530579) 成功；公开 `v0.8.5` 标签仍固定原 `7736dce`，未改产物。未来候选仍须完成自身的代码更新/失败恢复门禁。

这些事实来自源项目公开发行及基线验收交接，本站未新增原生菜单截图、真实模型调用或手机验收，也未替代源项目的安装验证。

## 内容影响与完成清单

| 文件与位置                                                                          | 正式同步结果                                                                                                                                             |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Fn 安装](../../content/projects/fn/docs/installation.md)，`macOS 安装`             | 默认切到真实公开 0.8.5/build 21 DMG / Release；补大小和 SHA-256；合并原候选说明，明确 0.8.4 及更早全部完整安装、保存编辑/退出/替换和数据保留             |
| 同文件，`应用内更新` / `源码开发`                                                   | 去除“0.8.3 可应用内更新到本版”的默认建议；说明 0.8.5 新基线实证和后续候选独立门禁；readiness 使用本版共仓修复，不改变外网代理/API 或独立 FIA npm         |
| [Fn 项目首页](../../content/projects/fn/index.mdx)，`TypeScript 应用开发`           | 切到 0.8.5，说明 1200px 居中、状态栏置顶与一次完整 DMG/数据保留；SDK/UI/翻译不变                                                                         |
| [第一次使用](../../content/projects/fn/docs/getting-started.md)，`桌面布局与状态栏` | 移除未公开预告；说明工具栏/列表外壳最大 1200px，窄窗规则不变；正式状态栏「显示桌面」、常用应用、系统操作及退出分组                                       |
| [排查](../../content/projects/fn/docs/troubleshooting.md)，`升级到 0.8.5`           | 完整 DMG 原因、数据保留及本机 readiness 修复；保留 0.8.3 About 缺陷的历史说明，不将其误写成本版在线更新路径                                              |
| [应用开发](../../content/projects/fn/docs/plugin-development.md)，首段              | 推荐宿主 0.8.5，保留 SDK 0.8.0 的最低已验证协议宿主 0.8.4；init 模板精确版本沿用 SDK 0.8.0/UI 0.2.2，源码 `commands/plugin.ts` 已核对，不改 npm 安装命令 |
| [FIA 开发](../../content/projects/fia/docs/development.md)，readiness 段            | 以 `7736dce` 固定共仓框架 TypeScript 修复；readiness 与既有网关直连分开，不改业务 API/外网代理，独立 npm 0.18.0 仍不包含                                 |
| [FIA 分发](../../content/projects/fia/docs/distribution.md)，0.8.5 段               | 正式产物、真实 Swift 原因、完整安装/数据保留、新基线登记及独立复用分别说明；不改变 FIA npm 状态                                                          |
| [项目资料](../project-sources.md)，首节                                             | 登记精确源码、candidate run/attempt/artifact、正式 run、匿名产物和公网报告、新基线摘要；历史 0.8.4 及此前证据不全局替换                                  |
| [内容发布](../content.md)，`Fn 平台文档维护`                                        | 记录公开验证后才切默认、公开/源码/基线/官网部署分开，以及仍未部署的状态                                                                                  |

来源为 Fn `93aecd1` 的 `frontend/desktop.module.css`、`974cd70` 的 FIA `FIAApplication.swift` / 原生菜单测试，以及最终 `7736dce` 的 `isDevelopmentServerReady` 与 `development.test.ts`。SDK、模型协议、摘要、排序、清单、市场教程和手机版本未改变；网站无源码、路由、数据库、依赖、工具链或部署保护变更。sitemap 由内容生成器更新，不手改生成文件。

## 本地 QA

准备阶段已实际运行 `bun run test`：51 通过、0 失败、175 断言；`bun run test:workers`：12 通过、0 失败。Workers 初轮用户级 Wrangler 日志写入 EPERM 提示已如实记录，测试 exit 0。正式切换只改变文档/元数据，未改测试或被测源码，按本次指令沿用这些结果，不冒充再次执行。

| 正式内容检查                                                 | 结果                                                                                                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 内容/技术清单生成与 `git diff --check`                       | 通过：35 篇内容、技术清单一致，生成 sitemap 更新，diff 无空白错误                                                                          |
| `bun run check`                                              | 通过：lint 0 warning/error、format、strict TS、35 篇内容、文档链接与生成一致性                                                             |
| `bun run build` / `bun run build:check`                      | 通过：正式内容 2431 客户端/3199 SSR 模块，42 页预渲染（含 `/admin`），41 公开页面/Home 导航/锚点/草稿隔离/项目搜索                         |
| 源码及实际 HTML 正式边界断言                                 | 通过：7 页源码/HTML 无本版未公开预告；精确 0.8.5 链接/大小/哈希、完整安装/数据保留/版本不变；24 正文可提取；0.8.4 及更早历史来源逐字节不变 |
| 工作流外 staging / production smoke 与 24 篇 Fn/FIA 正文比对 | 通过：部署后两环境各自 41 公开路径、RSS/sitemap/healthz、404、安全头、认证/后台缓存隔离与生产 www 跳转通过；对照提交 `1ccb543` 构建快照逐篇比对 24 篇完整 article 正文、canonical 与标题，逐项 SHA-256 一致 |

### 受限本机构建方式

准备阶段的预渲染 500 已捕获原始 Response 定位：Miniflare `DevRegistry.writeWorkerDefinition` 向沙箱外用户级 `.wrangler/registry/semicoder` 写入遭 EPERM，发生在应用渲染前，不是正文或数据库查询错误。既有本地 D1 文件存在，未修改或初始化数据库。详细原始堆栈仅保存在本机忽略日志，不复制到公开正文。

依赖实现支持 `WRANGLER_REGISTRY_PATH` / `MINIFLARE_REGISTRY_PATH`。只为构建进程指定项目内 registry/log，保持 registry 功能、原代理和严格预渲染门禁；使用已安装 Node 24.5.0，不修改源码、依赖、工具链、全局环境或权限。复跑方式（PATH 选择 Node 24）：

```sh
WRANGLER_LOG_PATH="$PWD/.wrangler/qa-logs" \
WRANGLER_REGISTRY_PATH="$PWD/.wrangler/qa-registry" \
MINIFLARE_REGISTRY_PATH="$PWD/.wrangler/qa-registry" \
bun run build && bun run build:check
```

## 部署与线上验收结果

1. 源项目 main `3b2b580`、[远端 CI 41554530579](https://github.com/semicoder-dev/fn/actions/runs/41554530579)、新锁及默认 code 恢复已核对；公开标签 `v0.8.5` 及候选原始产物保持 `7736dce`，未重建或重签。
2. 本站最终内容 `1ccb5431aff61aebd44b28eb3cb5894dad3b9a54` 已提交、快进合入 main 并推送；[工作流 41555914459](https://github.com/semicoder-dev/semicoder/actions/runs/41555914459) 的 Quality checks、Deploy staging、Deploy production 全部成功。生产部署按本次明确授权经既有 environment 审核批准，其他保护未改。
3. 已在 `https://staging.semicoder.dev` 与 `https://semicoder.dev` 分别完成工作流外全站 strict smoke，并对照精确提交的构建快照逐篇核对 24 个 Fn/FIA article 完整正文、canonical 与标题，全部逐字节一致；浏览器复核安装页精确 DMG 链接返回 200、`content-length` 45,846,716。
4. 线上正文、下载入口与两环境部署结果已登记到 [项目资料](../project-sources.md) 与[内容发布](../content.md)。未实际执行的登录、生产数据写入、手机或原生菜单截图不声明通过。
