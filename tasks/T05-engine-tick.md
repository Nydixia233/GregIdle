# T05：engine tick 纯函数 + Vitest

> TODO 回链：[`../TODO.md`](../TODO.md) T05
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`引擎`
> 文件锁：`src/engine/*.ts` / `vitest.config.ts`

---

## 目标

搭建 `src/engine/` 纯函数 tick 骨架（在线/离线共用），接 Vitest，覆盖核心数值正确性。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §5 数据模型 / §6 核心循环 / §6.3 电压效率
- [docs/data-sourcing.md](../docs/data-sourcing.md)（电压/超频数值出处）
- [AGENTS.md](../AGENTS.md) §4 架构约束（引擎纯函数无 I/O）
- 依赖 T03（Decimal）、T09（数值草表——引擎的「米」，单测用真实配方而非假数据）
- 时间单位：1 tick = 1 秒（已定，见设计文档 §12）

## 执行步骤

- [ ] 认领工单。
- [ ] `tick.ts` 纯函数：输入状态 + deltaTime → 新状态，无 I/O。
- [ ] `recipe.ts` / `era.ts` / `voltage.ts` / `offline.ts` / `quest.ts` 骨架。
- [ ] `offline.ts` 复用 tick，仅步长不同（定步长，见 §6.2）。
- [ ] `vitest.config.ts` + 核心单测：tick 推进 / 超频倍率 / 离线结算自洽。
- [ ] DoD 自检。

## DoD

- [ ] 引擎无任何 I/O（不读 store/localStorage/DOM）。
- [ ] tick 推进 / `getVoltageEffect` / 离线结算单测通过。
- [ ] 离线与在线用同一份 tick（数值一致性测试）。
- [ ] 电压/超频数值标注出处（data-sourcing.md）。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
