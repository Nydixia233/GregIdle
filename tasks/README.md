# 任务工单索引 (Tasks)

> `tasks/` 是 [TODO.md](../TODO.md) 的执行层：TODO 管全局看板（一行一个 T），工单文件记录每一项的目标、认领、文件锁、DoD 和产出链接。
> 宏观路线以设计文档 [§11](../docs/gtnh-idle-game-design.md) 为准；多窗口并行前先读 [AGENTS.md §7 多窗口协作协议](../AGENTS.md)。

---

## 使用规则

1. **先读 P0**：每轮开工先读 [AGENTS.md](../AGENTS.md) 的 P0 必读清单。
2. **先认领再动手**：在 [TODO.md](../TODO.md) 和对应工单里同步填写状态与认领窗口。
3. **按文件锁执行**：同一文件不允许多个窗口同时写不同部分；会碰同一文件的任务拆成不同波次或先协调文件锁。
4. **状态双向同步**：TODO 行、工单文件必须保持一致。
5. **编号规则**：完成（🟢）与进行中（🟡）的工单编号稳定，不重排；删除某工单时其后**未开始（⬜）**的可顺次前移补空号，被删编号不复用；新增从当前最大号 +1 继续。
6. **工单按需建**：小任务（一次会话内完成）只活在 TODO.md 一行即可；**只有跨会话、多步骤、需追踪产出的任务**才从 [`_template.md`](_template.md) spin 一个工单文件。

## 状态图例

| 标记 | 含义 |
|------|------|
| 🟢 完成 | 满足工单 DoD，已同步 TODO |
| 🟡 进行中 | 已认领或已有骨架，仍需补 |
| 🔴 占位 | 已登记，暂不展开 |
| ⬜ 未开始 | 尚未认领 |

---

## 工单索引

### Phase 0 · 骨架搭建

| 编号 | 工单 | 状态 | 认领窗口 |
|------|------|------|----------|
| T01 | 治理 / 记忆文档体系 | 🟢 完成 | 主控 |
| T02 | [Vite + React + TS + Tailwind 初始化](T02-scaffold-init.md) | ⬜ 未开始 | — |
| T03 | [接入 break_infinity.js 数值底座](T03-decimal-base.md) | ⬜ 未开始 | — |
| T04 | [SaveEnvelope + migrate() 骨架](T04-save-envelope.md) | ⬜ 未开始 | — |
| T05 | [engine tick 纯函数 + Vitest](T05-engine-tick.md) | ⬜ 未开始 | — |
| T06 | [Zustand store + persist](T06-store-persist.md) | ⬜ 未开始 | — |
| T07 | [ESLint + Prettier 配置](T07-lint-format.md) | ⬜ 未开始 | — |
| T08 | [CI workflow](T08-ci.md) | ⬜ 未开始 | — |

### Phase 1 · 最小可玩版（T09-T19）

> 工单按需 spin（规则 6）。当前只在 [TODO.md](../TODO.md) 登记，未展开为工单文件。

### Phase 2 · 深度填充（T20-T27）

> 同上，按需展开。

### Phase 3 · 中后期扩展（T28-T35）

> 同上，按需展开。

---

_工单文件命名 `T0X-简述.md`（kebab-case）。完成后保留文件作存档，状态置 🟢。_
