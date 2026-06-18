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

- [x] 设计文档 v1.3（8 条有效问题修正）
- [x] GitHub 仓库创建并推送
- [x] `.gitignore` + `.gitattributes`
- [x] 治理文档：AGENTS.md / VERSION.md
- [x] docs：data-sourcing / conventions / glossary / errors（README + 模板）

## 下一步

**Phase 0：项目脚手架**（设计文档 §11，单独开一轮）

- [ ] Vite + React + TypeScript + Tailwind 初始化
- [ ] 接入 `break_infinity.js`（Decimal）作为数值底座 —— 先定，渗透最广
- [ ] `SaveEnvelope` schema 版本头 + `migrate()` 骨架 —— 与 Decimal serialize 一起定
- [ ] `src/engine/` tick 纯函数骨架 + Vitest 核心单测 + `vitest.config.ts`
- [ ] Zustand store + persist（含 Decimal 自定义 serialize/deserialize）
- [ ] 目录结构按设计文档 §3 落地
- [ ] ESLint + Prettier 配置（AGENTS.md §4 已引用"以配置为准"，需落地）
- [ ] `.github/workflows/ci.yml`：lint + typecheck + test + build（Web 项目 CI 成本低、收益即时）

> Phase 0 依赖顺序：**大数底座 + SaveEnvelope（渗透最广）→ 引擎 tick + Vitest → store/persist**，避免回头改序列化。
> ESLint/Prettier/CI/Vitest 是脚手架的自然组成部分，与 Phase 0 一起建，不算额外治理。

## 治理文档「何时建什么」触发清单

> 原则：**治理文档密度匹配代码密度**。以下文件现在建只能填占位符，到触发条件出现（有真实内容可填）再建。避免空壳先腐烂。

| 文件 | 触发条件 |
|------|----------|
| `TODO.md` / 中央工作队列 | 任务多到 STATUS「下一步」列表装不下时 |
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
