# 命名与组织约定 (Conventions)

> 统一命名与文件组织，减少 diff 噪音和歧义。代码风格的总规则见 [AGENTS.md §4](../AGENTS.md)，本文件补充具体规则与示例。

---

## 1. 游戏数据 id（snake_case）

所有 `src/data/` 里的实体 id 用小写下划线，全局唯一、稳定（一旦写进存档就不能随意改，改了要走存档迁移）。

| 实体 | 规则 | 示例 |
|------|------|------|
| 资源 / 材料 | `<材料>_<形态>` | `iron_ingot`、`copper_wire`、`rubber_bar`、`oxygen_gas` |
| 机器 | 全称转下划线 | `electric_blast_furnace`、`steam_macerator`、`chemical_reactor` |
| 配方 | `<产物>_from_<来源>` 或 `<动作>_<对象>` | `iron_ingot_from_ore`、`macerate_iron_ore` |
| 任务节点 | `<时代>_<简述>` | `stone_first_furnace`、`lv_first_machine` |
| 时代 | 小写 | `stone`、`steam`、`lv`、`mv`…（`Era` 类型，电压段用小写） |

> 形态词统一用设计文档 §5.3 的 `MaterialForm`：`ore / crushed / purified / dust / ingot / plate / rod / wire / fluid / gas / plasma`。

---

## 2. 代码标识符

| 种类 | 风格 | 示例 |
|------|------|------|
| 组件 / 类型 / 接口 | PascalCase | `MachineCard`、`PlayerState`、`VoltageEffect` |
| 变量 / 函数 / hook | camelCase | `getVoltageEffect`、`euStored`、`useGameLoop` |
| 常量 | UPPER_SNAKE | `STEP_MS`、`MAX_OFFLINE_MS`、`TICK_RATE` |
| 文件（组件） | 与导出同名 PascalCase | `MachineCard.tsx` |
| 文件（非组件） | camelCase | `tick.ts`、`offline.ts`、`format.ts` |

---

## 3. 目录组织

以设计文档 §3 的结构为准：

- `src/engine/` — 纯函数引擎，无 I/O（见 AGENTS.md §4 架构约束）。
- `src/data/` — 纯静态配置，不含逻辑。
- `src/stores/` — Zustand store。
- `src/components/<域>/` — 按域分组（`layout` / `resource` / `workshop` / `quest` / `common`）。
- `src/hooks/` `src/utils/` — 自定义 hook 与工具。

---

## 4. 文档与链接

- 文档文件名：kebab-case（`data-sourcing.md`、`gtnh-idle-game-design.md`）；错误文件 `ERROR-YYYY-MM-DD-简述.md`。
- 链接用相对路径的标准 Markdown `[文本](相对路径)`，不用 wikilink。
- 文档与代码中**禁止本地绝对路径**（铁律，AGENTS.md §0）。

---

## 5. Git 提交

`type: 简短描述` + 中文正文，type ∈ `feat / fix / docs / chore / refactor / style / test`。详见 [AGENTS.md §3](../AGENTS.md)。
