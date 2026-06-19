# 当前状态 (STATUS)

> 记「现在干到哪、下一步、卡在哪」。频繁更新，自由改（区别于 AGENTS.md 的稳定规则、设计文档 §11 的静态路线图）。
> 路线图回答「整体计划」，本文件回答「此刻」。

---

## 当前阶段

**设计定稿 + 治理文档搭建** —— 尚未进入 Phase 0 编码。

- 设计文档 `docs/gtnh-idle-game-design.md` 已到 v1.3（修正离线结算 / 大数 / 节奏三处架构隐患，受众收窄为 GTNH 熟人）。
- 治理 / 记忆文档体系已建（AGENTS / VERSION / data-sourcing / conventions / glossary / errors / 本文件）。
- 仓库：https://github.com/Nydixia233/GregIdle（public，main 分支）。

## 已完成

- [x] T01 设计文档 v1.3（8 条有效问题修正）+ 治理文档体系
- [x] GitHub 仓库创建并推送
- [x] `.gitignore` + `.gitattributes`
- [x] 治理文档：AGENTS.md / VERSION.md / TODO.md
- [x] docs：data-sourcing / conventions / glossary / errors（README + 模板）

> 完整任务清单见 [TODO.md](TODO.md)（Phase 下挂 `T+编号`，全局连续）。本文件只记此刻位置与卡点，不再重复列任务。

## 开工前已定的硬约束（2026-06-19）

| 决定 | 值 | 影响 |
|------|-----|------|
| 包管理器 | **pnpm** | T02 起所有命令 |
| 时间单位 | **1 tick = 1 秒** | 所有 `baseDuration` / 产率的数值基准（设计文档 §6.1） |
| Phase 1 数值表 | **前置为 T09，先于引擎落表** | T05 引擎有真实数据可跑 |
| 部署目标 | **GitHub Pages** | T08 CI deploy |

## 下一步

**Phase 0：项目脚手架** —— 见 [TODO.md](TODO.md) 的 `T02`～`T09`。

依赖顺序：**T02 脚手架（pnpm）→ T03 大数底座 + T04 SaveEnvelope（渗透最广）→ T09 数值草表（引擎的米）→ T05 引擎 tick + Vitest → T06 store/persist**，避免回头改序列化。T07 ESLint/Prettier、T08 CI 是脚手架自然组成，与初始化一起建。

## 治理文档「何时建什么」触发清单

> 原则：**治理文档密度匹配代码密度**。以下文件现在建只能填占位符，到触发条件出现（有真实内容可填）再建。避免空壳先腐烂。

| 文件 | 触发条件 |
|------|----------|
| ~~`TODO.md`~~ | ✅ 已建（任务清单，Phase 下挂 T） |
| `CHANGELOG.md` | 第一个可发布版本（Phase 1 完成、有可玩 build）时按版本号组织，给玩家看。当前"开发日志"职责由 git commit 正文承担，不提前建 |
| `docs/workflow.md`（开发 SOP） | 出现「反复做错的固定流程」，从 errors 提炼时——不凭空写 |
| `templates/`（组件/store/test 模板） | 建到第 3-4 个同类文件、确实在复制粘贴时 |
| `docs/ui-mockups/` HTML 原型 | 某个复杂 UI 写 React 前需要先验证交互时（可选，ASCII 布局够用就跳过） |
| `src/locales/` i18n | Phase 3（设计文档已定，受众是中文 GTNH 熟人） |
| `docs/verification-checklist.md` | 不建——GregIdle 的等价物是 Vitest 单测 |
| `docs/project-charter.md` / `docs/roadmap.md` | 不建——设计文档 §1/§11/§12 已承担，拆出只增同步负担 |

## 卡点 / 待决策

- LICENSE 暂未放（当前用 README 非官方声明替代），公开发布前需定授权类型。
- `OverclockCalculator` 的确切源码行号待补（data-sourcing.md 第 2 节标「待补」）。

---

_更新约定：阶段推进、下一步变化时改本文件；踩坑记 `docs/errors/`；长期规则才进 AGENTS.md。_
