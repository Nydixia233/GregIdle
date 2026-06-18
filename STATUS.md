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
- [ ] `src/engine/` tick 纯函数骨架 + Vitest 核心单测
- [ ] Zustand store + persist（含 Decimal 自定义 serialize/deserialize）
- [ ] 目录结构按设计文档 §3 落地

> Phase 0 依赖顺序：**大数底座 + SaveEnvelope（渗透最广）→ 引擎 tick + Vitest → store/persist**，避免回头改序列化。

## 卡点 / 待决策

- LICENSE 暂未放（当前用 README 非官方声明替代），公开发布前需定授权类型。
- `OverclockCalculator` 的确切源码行号待补（data-sourcing.md 第 2 节标「待补」）。

---

_更新约定：阶段推进、下一步变化时改本文件；踩坑记 `docs/errors/`；长期规则才进 AGENTS.md。_
