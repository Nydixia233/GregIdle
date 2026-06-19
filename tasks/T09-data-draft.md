# T09：Phase 1 数值草表（资源/机器/配方）

> TODO 回链：[`../TODO.md`](../TODO.md) T09
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`数据`
> 文件锁：`docs/data-phase1-draft.md`（新建）/ 后续 `src/data/*.ts`

---

## 目标

定出 Stone→Steam→LV 入门段的具体数值表——资源、机器、配方的实际数字，作为引擎（T05）的「米」。
**前置于 T05**：引擎能算但没有真实配方就只能用假数据跑测试，写完还要返工对接。先有数据，引擎一写完即可喂真值。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §5 数据模型 / §7 时代进阶 / §10 数据规模
- [docs/data-sourcing.md](../docs/data-sourcing.md) 溯源格式与已锚定清单
- [VERSION.md](../VERSION.md) 锚定版本
- `_local/config-gregtech/`（MachineStats 等实测数值基准）
- `_local/questbook/DefaultQuests/`（Stone/Steam/LV 任务节点 → 配方依赖）
- `_local/gt5u-src/`（关键数值标 `文件:行号`）
- [TODO.md](../TODO.md)

## 全局约束（已决策，写表时遵守）

- **时间单位：1 game tick = 1 秒**。所有 `baseDuration` / `baseSpeed` 以秒为单位填写。
- 数值统一 Decimal 语义（落 `src/data/` 时用 `decimal.ts` 出口）。
- 每个关键数值（电压、超频倍率、蒸汽换算、配方耗时基准）标 GT5U 出处或 config 来源；放置简化值标注「GregIdle 调参」。

## 执行步骤

- [ ] 认领工单。
- [ ] 从 questbook 提取 Stone/Steam/LV 主线节点，列出涉及的资源与机器。
- [ ] 资源表：~20-30 项（id / 名称 / form / era / 是否流体）。
- [ ] 机器表：~8-12 项（id / role / tier / 耗能 / baseSpeed 秒 / 配方槽）。
- [ ] 配方表：~30-45 条（inputs / outputs / byproducts / baseDuration 秒 / era / 出处）。
- [ ] 标注每条数值的来源（GT5U 文件:行号 / config / GregIdle 调参）。
- [ ] 按 §7.1.1 节奏曲线（Stone ~10 分钟、Steam ~1 小时、LV ~半天有效时间）粗校产率，确认不离谱。
- [ ] DoD 自检。

## DoD

- [ ] 三张表落到 `docs/data-phase1-draft.md`，数量达标（资源 20-30 / 机器 8-12 / 配方 30-45）。
- [ ] 每条关键数值有来源标注；放置简化处明确标「调参」。
- [ ] 单位统一为秒（1 tick=1 秒），与引擎约定一致。
- [ ] 配方依赖闭合：每个 input 资源都有产出它的配方或采集点（无悬空材料）。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
