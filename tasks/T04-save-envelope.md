# T04：SaveEnvelope + migrate() 骨架

> TODO 回链：[`../TODO.md`](../TODO.md) T04
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`引擎`
> 文件锁：`src/utils/storage.ts` / `src/utils/save.ts`（新建，SaveEnvelope 类型 + migrate + serialize 逻辑）

---

## 目标

存档带 `schemaVersion` 头，预留逐级迁移；Decimal 字段自定义 serialize/deserialize。与 T03 的大数底座绑定，须一起定。
逻辑（`migrate`/serialize）放 `src/utils/`，不放 `src/data/`——后者按 AGENTS.md §4 只存纯静态配置。`SaveEnvelope` 是存档系统局部类型（§9），随逻辑放 `src/utils/save.ts`，不进 T36 的核心域类型 `src/types.ts`。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §9 存档机制（含「存档迁移」节）
- [TODO.md](../TODO.md) / 依赖 T03（Decimal）、T36（`PlayerState` 等类型——`SaveEnvelope.gameState` 即 `PlayerState`）

## 执行步骤

- [ ] 认领工单。
- [ ] 定义 `SaveEnvelope { schemaVersion, timestamp, gameState, checksum }`。
- [ ] `checksum`：用简单非加密哈希（如 FNV-1a / djb2）对序列化后的 `gameState` 算值，仅用于检测手改 / 存档损坏，不用于安全。
- [ ] Decimal 自定义 serialize（→ 字符串）/ deserialize。
- [ ] `migrate(save)`：按版本顺序逐级迁移骨架（当前仅 v1）。
- [ ] 读到高于当前版本 → 拒绝加载并提示。
- [ ] Vitest：v1 存档读写往返 + Decimal 字段不失真。
- [ ] DoD 自检。

## DoD

- [ ] v1 存档读写往返测试通过。
- [ ] Decimal 字段序列化/反序列化无精度损失。
- [ ] 高版本存档被拒绝（不静默吞档）。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
