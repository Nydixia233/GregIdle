# T36：共享类型定义 src/types.ts

> TODO 回链：[`../TODO.md`](../TODO.md) T36
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`引擎`
> 文件锁：`src/types.ts`（新建，全局共享类型出口）

---

## 目标

建立全项目共享的 TypeScript 类型定义（设计文档 §5 的数据模型），作为 `data` / `engine` / `stores` 三层共同引用的单一出口。**逻辑上属 Phase 0，执行顺序紧跟 T03、前置于 T04/T05/T06**（编号按全局 +1 规则取 T36，不重排已有工单）。否则三层各自定义类型、互相打架。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §5 核心数据模型（全部 interface / type）
- [AGENTS.md](../AGENTS.md) §4 代码规范（数值类型约定：累积量 Decimal / 配方系数 number）
- [TODO.md](../TODO.md) / 依赖 T03（Decimal 定了才能定 `resources` 等字段类型）

## 执行步骤

- [ ] 认领工单，更新 TODO 与本文件状态。
- [ ] 把设计文档 §5 的类型誊进 `src/types.ts`：`Era` / `VoltageTier` / `MaterialForm` / `Resource` / `MachineRole` / `MachineDef` / `PlayerMachine` / `Recipe` / `ActiveRecipe` / `QuestNode` / `QuestRequirement` / `QuestReward` / `EnergyState` / `EnergyDelta` / `PlayerState`。
- [ ] 严格执行 §5.9 数值类型约定：累积量 / 持有量 / 速率用 `Decimal`（`resources`、`totalProduction`、`*Stored`、`*Capacity`）；配方系数（`inputs`/`outputs`/`byproducts`）与计时量（时间戳）保持 `number`。
- [ ] `Decimal` 从 T03 的 `src/utils/decimal.ts` 出口 import，不直接依赖 `break_infinity.js`。
- [ ] DoD 自检。

## DoD (Definition of Done)

- [ ] §5 全部类型在 `src/types.ts` 就位，`tsc --noEmit` 通过。
- [ ] 数值字段类型符合 §5.9 约定（累积量 Decimal / 系数与时间戳 number）。
- [ ] 无裸 `any`；导出供 data/engine/stores 引用。
- [ ] 本地 Markdown 链接可解析。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
