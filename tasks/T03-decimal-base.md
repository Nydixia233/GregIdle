# T03：接入 break_infinity.js 数值底座

> TODO 回链：[`../TODO.md`](../TODO.md) T03
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`引擎`
> 文件锁：`package.json` / `src/utils/format.ts` / `src/utils/decimal.ts`（新建）

---

## 目标

接入 `break_infinity.js`，确立 Decimal 为游戏数值的唯一类型（贯穿引擎与存档），并实现大数格式化。渗透最广，须在引擎/存档之前定。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §2 技术选型（大数行）/ §13 风险表（大数溢出）
- [AGENTS.md](../AGENTS.md) §4 架构约束（数值统一走 Decimal）
- [TODO.md](../TODO.md)

## 执行步骤

- [ ] 认领工单。
- [ ] 安装 `break_infinity.js`（pin 版本）。
- [ ] 封装 `src/utils/decimal.ts`：统一 import 出口 + 常用构造/运算 helper。
- [ ] `src/utils/format.ts`：大数格式化 K/M/B/T/Qa/Qi/Sx…
- [ ] 写 Vitest 单测覆盖格式化边界（0、负、极大、小数）。
- [ ] DoD 自检。

## DoD

- [ ] `format.ts` 单测通过（含边界值）。
- [ ] 项目内**无界游戏量**（累积量/持有量，见设计文档 §5.9）统一从 `decimal.ts` 出口取用，不裸用 number。有界量（配方系数、瞬时增量、时间戳）按 §5.9 保持 number。
- [ ] 版本号已 pin。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
