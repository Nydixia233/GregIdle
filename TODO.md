# 任务清单 (TODO)

> 所有子任务的落点。按 Phase 分节，任务编号 `T+编号` **全局连续、跨 Phase 唯一、不重置**。
> 状态：`[ ]` 待办 · `[~]` 进行中 · `[x]` 完成（标完成日期）。
> 每个任务带**目标**与**验收**。完成时在此打勾，并在该次 commit 正文写清做了什么（commit 即开发日志，见 AGENTS.md §3）。
> 当前进度快照见 [STATUS.md](STATUS.md)；本文件管「待办全集」，STATUS 管「此刻」。

---

## Phase 0 · 骨架搭建

- [x] **T01 治理 / 记忆文档体系** （完成 2026-06-19）
      目标：AGENTS / VERSION / STATUS / data-sourcing / conventions / glossary / errors 就位
      验收：文档互链无死链，`_local/` 正确忽略，已推送 main ✓

- [ ] **T02 Vite + React + TypeScript + Tailwind 初始化**
      目标：`npm run dev` 能起空白页；目录结构按设计文档 §3
      验收：dev server 启动无报错，TS 严格模式开

- [ ] **T03 接入 break_infinity.js 数值底座**
      目标：Decimal 贯穿引擎与存档，确定唯一数值类型
      验收：`format.ts`（大数格式化 K/M/B/…）单测通过

- [ ] **T04 SaveEnvelope schema 版本头 + migrate() 骨架**
      目标：存档带 `schemaVersion`，预留逐级迁移
      验收：v1 存档读写往返测试通过；Decimal 自定义 serialize/deserialize 可用

- [ ] **T05 src/engine/ tick 纯函数骨架 + Vitest**
      目标：在线/离线共用的纯函数 tick；`vitest.config.ts` 就位
      验收：tick 推进 / 超频倍率 / 离线结算数值自洽的核心单测通过

- [ ] **T06 Zustand store + persist**
      目标：核心 game store，persist 接 T04 的 serialize
      验收：状态变更触发 selector 重渲染；刷新页面存档不丢

- [ ] **T07 ESLint + Prettier 配置**
      目标：落地 AGENTS.md §4 引用的「以配置为准」
      验收：`npm run lint` 通过，规则覆盖 TS/React

- [ ] **T08 CI：.github/workflows/ci.yml**
      目标：push 自动跑 lint + typecheck + test + build
      验收：首次 push 后 Actions 全绿

---

## Phase 1 · 最小可玩版

> Stone → Steam → LV 入门，第一版终点：造出第一台 LV 电力机器（设计文档 §11）。

- [ ] **T09 三栏布局 + TopBar / BottomBar 框架**
- [ ] **T10 ResourcePanel + ResourceRow 组件**
- [ ] **T11 WorkshopPanel + MachineCard + RecipeSelector 组件**
- [ ] **T12 QuestBookPanel + QuestEraTabs + EraRoadmap 组件**
- [ ] **T13 useGameLoop tick 主循环（rAF）**
- [ ] **T14 useOfflineCalc 离线结算（复用引擎纯函数）**
- [ ] **T15 Stone/Steam/LV 数据：20-30 资源 + 8-12 机器 + 30-45 配方**
      验收：数值标注出处（data-sourcing.md），关键数对照 GT5U 源码
- [ ] **T16 前 5 分钟手动采集 → 解锁自动采集点 + 工作台队列**
- [ ] **T17 工业仪表盘 CSS 主题（§4.6 变量）**
- [ ] **T18 存档/读档 + 导出/导入**
- [ ] **T19 第一版终点验收：通关到第一台 LV 电力机器**

---

## Phase 2 · 深度填充

> LV → MV → HV 扩展（设计文档 §11）。

- [ ] **T20 LV→MV→HV 电压扩展**
- [ ] **T21 资源/材料扩到 50+，机器 20 种**
- [ ] **T22 基础化学线（橡胶/酸/基础流体）**
- [ ] **T23 完整材料加工链（ore→…→wire）**
- [ ] **T24 副产物管理系统**
- [ ] **T25 时代任务图扩展到 HV**
- [ ] **T26 数据统计面板（总产量/效率榜/历史）**
- [ ] **T27 ProgressBar / PowerGauge 动画打磨**

---

## Phase 3 · 中后期扩展（持续迭代）

- [ ] **T28 EV→IV→…→UXV 分阶段扩展**
- [ ] **T29 高阶自动化、AE/批量队列、终局材料**
- [ ] **T30 framer-motion 动画（数字跳动/进度发光/指示灯）**
- [ ] **T31 移动端响应式适配**
- [ ] **T32 数值平衡调优（按 §7.1.1 节奏曲线校准）**
- [ ] **T33 成就系统**
- [ ] **T34 i18n 国际化（中/英）**
- [ ] **T35 CHANGELOG.md**（第一个可发布版本时建，按版本号组织，给玩家看）

---

_新任务追加到对应 Phase 末尾，编号接最大 T 继续递增。_
