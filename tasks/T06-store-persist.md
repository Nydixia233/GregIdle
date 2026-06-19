# T06：Zustand store + persist

> TODO 回链：[`../TODO.md`](../TODO.md) T06
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`引擎`
> 文件锁：`src/stores/*.ts`

---

## 目标

建核心 game store，persist 接 T04 的 serialize；selector 精准重渲染。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §5.9 PlayerState / §2 状态管理 / §9 存档
- 依赖 T03（Decimal）、T36（共享类型 `src/types.ts`）、T04（SaveEnvelope serialize）
- [TODO.md](../TODO.md)

## 执行步骤

- [ ] 认领工单。
- [ ] `gameStore.ts`（资源/机器/任务/tick）、`uiStore.ts`、`settingsStore.ts`。
- [ ] persist 中间件接 T04 的自定义 serialize/deserialize。
- [ ] 每 30s 自动写入（§9）。
- [ ] DoD 自检。

## DoD

- [ ] 状态变更触发 selector 重渲染（非全量）。
- [ ] 刷新页面存档不丢，Decimal 字段还原正确。
- [ ] store 不混入引擎逻辑（逻辑在 engine，store 只持状态）。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
