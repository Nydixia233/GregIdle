# 协作规范 (AGENTS.md)

> 本文件是 GregIdle 项目的行为准则，AI 每轮对话开始必读。
> **本项目是一个 GTNH 主题的网页放置游戏（React / TypeScript / Vite，纯前端单机）。**
> 修改本文件**必须经用户确认**，不可擅自更改。

---

## 0. 三条铁律

| 铁律 | 含义 | 落地 |
|------|------|------|
| **数值溯源** | 任何来自 GTNH 的数值（电压、超频、蒸汽换算、材料归属）必须可回溯到锁定版本，标 `gt5u-src` 文件:行号 | [VERSION.md](VERSION.md) · [docs/data-sourcing.md](docs/data-sourcing.md) |
| **禁绝对路径** | 入库文档与代码中**不得出现本地绝对路径**（盘符、用户目录）。本地路径只存在于本机环境，不进仓库 | 全仓库 |
| **`_local/` 永不入库** | `_local/` 是版权 / 大体积参考资料工作台，除其自身 README 外全部 git 忽略；不直推 main 之外的破坏性操作、不强推 | [.gitignore](.gitignore) · [_local/README.md](_local/README.md) |

违反铁律的产出一律视为未完成。

---

## 1. 项目速览

- **定位**：GTNH 从石器到 UXV 的长线科技推进，做成网页放置 / 挂机游戏（Idle / Incremental）。不是 1:1 复刻，而是「放置翻译」。
- **受众**：熟悉 GTNH 的社区玩家（已知 EBF / Cleanroom / Naquadah 等术语）。非 GTNH 背景玩家不是第一版主受众，不内建术语扫盲层。
- **技术栈**：React 18 + TypeScript · Vite · Tailwind CSS · Zustand(persist) · framer-motion · break_infinity.js(Decimal) · Vitest。
- **数值锚**：GTNH 2.9.0-beta-1 / GT5-Unofficial 5.09.52.594（见 VERSION.md）。
- **非商业**：fan project，非官方、不收费、不做广告 / 账号体系。

---

## 2. 必读清单

每轮开场顺这张表读，按需深入。**只读指针，不在本文件复制会过期的副本。**

| 优先级 | 文件 | 为什么读 |
|--------|------|----------|
| **P0** | `AGENTS.md`（本文件） | 铁律、协作规范、代码规范 |
| **P0** | [docs/gtnh-idle-game-design.md](docs/gtnh-idle-game-design.md) | 设计总纲：定位、数据模型、时代进阶、路线图 |
| **P0** | [VERSION.md](VERSION.md) | 数值锚定单一事实源 |
| **P0** | [STATUS.md](STATUS.md) | 当前进度、卡点（频繁更新） |
| **P0** | [TODO.md](TODO.md) | 任务清单：Phase 下挂 `T+编号`，认领/打勾在此 |
| **P0** | [docs/errors/README.md](docs/errors/README.md) | 已踩过的坑，避免重复 |
| P1 | [docs/data-sourcing.md](docs/data-sourcing.md) | 写数据 / 改数值前看：溯源格式与已锚定清单 |
| P1 | [docs/conventions.md](docs/conventions.md) | 命名 / id / 文件组织约定 |
| P1 | [docs/glossary.md](docs/glossary.md) | GTNH 术语 → 游戏内作用 |

> 阅读顺序：**设计文档（做什么）→ STATUS（做到哪）→ TODO（做哪件 T）→ 动手**。

---

## 3. Git 提交规范

- 里程碑式提交，保持工作区整洁。
- 标题：`type: 简短描述`（type ∈ `feat` / `fix` / `docs` / `chore` / `refactor` / `style` / `test`）。
- 正文：中文 Markdown，记录改动范围、核心变更、影响。标题与正文空行分隔。
- 遵守 git 安全：不直推 main 之外的破坏性操作、不强推；敏感文件（含密钥）先确认；`_local/` 永不入库。

---

## 4. 代码规范（TypeScript / React）

### 命名
- 组件 / 类型 / 接口：大驼峰 `PascalCase`（`MachineCard`、`PlayerState`）。
- 变量 / 函数：小驼峰 `camelCase`（`getVoltageEffect`、`euStored`）。
- 常量：全大写下划线 `UPPER_SNAKE`（`STEP_MS`、`MAX_OFFLINE_MS`）。
- 游戏数据 id：小写下划线 `snake_case`（`iron_ingot`、`electric_blast_furnace`、`iron_ingot_from_ore`）。详见 conventions.md。

### 架构约束
- **引擎纯函数**：`src/engine/` 内不得有 I/O、不得读 store / localStorage / DOM；输入状态 → 输出新状态。在线 tick 与离线结算共用同一份。
- **数值统一走 Decimal**：资源量 / 速率 / 倍率一律用 `break_infinity.js` 的 Decimal，不混用原生 number 做游戏数值。
- **数据与逻辑分离**：`src/data/` 纯静态配置，不含逻辑。

### 注释与格式
- 类 / 接口 / 重要函数加中文注释，说明意图而非复述代码。
- 复杂逻辑加行内注释解释思路；改代码时同步更新注释。
- 2 空格缩进（前端约定），遵循项目 Prettier / ESLint 配置（Phase 0 落地后以配置为准）。

---

## 5. 错误记录规范

- 反复出现的问题、踩过的坑、重要教训，及时记入 `docs/errors/`。
- 索引文件 [docs/errors/README.md](docs/errors/README.md) 是指针文档，用二级标题指向具体 `ERROR-YYYY-MM-DD-简述.md`。
- 每条至少含：现象、触发场景、根因、修复、预防（五段式，模板见 `docs/errors/_TEMPLATE.md`）。

---

## 6. 协作模式

单一 Claude 协作（架构 + 实现 + 记录一体）。若未来引入其他工具协作，在此补角色分工，并经用户确认。
