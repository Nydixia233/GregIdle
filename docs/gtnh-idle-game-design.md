# GregIdle — 设计文档

> 创建日期：2026-06-19  
> 状态：已确认，待实现  
> 文档版本：v1.3 — 修正离线结算/大数/节奏三处架构隐患，受众收窄为 GTNH 熟人玩家  
> 最后一版修订日期：2026-06-19

---

## 一、项目定位

| 项 | 决策 |
|---|------|
| **名称** | GregIdle |
| **平台** | Web 网页游戏（纯前端，静态托管） |
| **类型** | 单机放置/挂机游戏（Idle/Incremental Game） |
| **主题** | GTNH（GregTech: New Horizons）——Minecraft 著名硬核科技整合包 |
| **深度** | 从石器时代到 UXV 的章节化长线推进：采集、蒸汽、电力、化学、副产物、后期高阶自动化 |
| **视觉** | 工业仪表盘风——深灰金属底色 + 暖黄发光 + 铜色渐变进度条 |
| **目标玩家** | 熟悉 GTNH 的社区玩家（已知道 EBF / Cleanroom / Naquadah 等术语）。非 GTNH 背景的放置玩家不是第一版主受众——文案与引导按熟人密度设计，不内建术语扫盲层 |
| **盈利模式** | 非商业 fan project，免费分享给朋友和社区，不做收费、广告或账号体系 |
| **素材边界** | 可使用原作/社区素材与自制素材，但公开发布前要逐项确认许可与署名 |

### 参考案例

| 游戏 | 参考维度 |
|------|---------|
| **Melvor Idle** | 架构模式：纯前端 + 存档 + 离线计算，硬核主题→放置翻译 |
| **NGU Industries** | UI 风格：工厂仪表盘、多资源加工链卡片、工业质感 |
| **Antimatter Dimensions** | Prestige 设计：多层重启 + 永久加成 + 指数增长节奏 |
| **Kittens Game** | 资源链深度：数十种资源嵌套依赖，无画面但深度足够 |
| **Factory Idle** | 产线思维：加工器组合 + 传送带逻辑 |
| **GTNH-Insight** | 设计数据源：GTNH 2.9.0-beta-1 全流程源码级验证知识库——含 Dev-Doc（16 阶段设计意图/材料/痛点）、源码片段、Wiki 交叉核对 |

> **真实性声明**：GregIdle 的数值设计以 GTNH 2.9.0-beta-1 / GT5-Unofficial 5.09.52.594 为锚定版本。电压档位、蒸汽换算、超频倍率、材料归属等核心数据均从 GT5-Unofficial 源码、GTNH Dev-Doc 与 Wiki 交叉核对后纳入。GregIdle 是"放置翻译"——在忠实于 GTNH 材料链与时代节奏的前提下，将数千条配方和数百台机器抽象为放置游戏可玩的规模。当放置简化与 GTNH 原作冲突时，优先保留 GTNH 的核心体验（副产物、电压机制、产线规划），而非数值的 1:1 复刻。

### 抽象边界

GregIdle 是 GTNH 的**放置游戏翻译**，不是 1:1 复刻。以下 GTNH 内容有意不建模：

| GTNH 内容 | GregIdle 处理 | 原因 |
|-----------|--------------|------|
| 生存/食物/饥饿 | 不建模 | 放置游戏核心是产线，不是生存 |
| 战斗/地牢/Boss | 不建模 | GTNH Dev-Doc 明确战斗聚焦前期生存与特定 Boss 战，放置游戏不做实时战斗 |
| 矿脉生成与找矿 | 抽象为自动采集点产出 | 放置玩家不需要"找矿"；GTNH 原矿脉网格识别（GT ore veins）为手动玩法 |
| 多方块结构搭建 | 抽象为"建造机器"按钮 | 简化空间搭建为点击操作；GTNH 多方块（如 Cleanroom、Assembly Line、EBF）保留为机器建造操作 |
| Thaumaturgy | 不建模 | GTNH Dev-Doc 明确"魔法不作为任何科技 tier 的主门控"——可不做魔法到达 Tech Tree 终点 |
| Blood Magic | 不建模 | HV 起可接触，但为纯魔法支线 |
| Botania | 不建模 | LV/MV 交界可入门，不锁定科技主线 |
| Witchery | 不建模 | MV 可接触，但 GTNH Dev 明确不再扩展此模组内容 |
| Bees (Forestry/Gendustry) | 不建模 | 魔法/农业支线，非核心科技树 |
| 航天/火箭/多星球 | 远期可选（Phase 3+） | GTNH 以火箭登月为 HV 终点、T2 火箭为 EV 终点、T3-T7 覆盖 IV-UV——作为 Prestige 层完美匹配 |
| 46 条 QuestLine 的支线任务 | 只保留主线科技章节 | 放置游戏不适合 3749 个任务 |
| 污染系统 | 远期可选 | GTNH Dev-Doc 有"Environment Crafting"远景——后期可作为环境压力→资源转换机制 |
| 管道/线缆/物流布线 | 抽象为资源自动流转 | 放置游戏不做空间物流 |
| 维护系统（Maintenance Hatch） | 远期可选 | GTNH 特色修复机制，可抽象为机器效率衰减+消耗品修复 |

> 真实 GTNH 任务书含 46 条 QuestLine、3749 个任务节点、5317 条前置依赖边。GregIdle 的 Quest Book 将其抽象为 ~80-150 个关键任务节点，只保留科技主线。

---

## 二、技术选型

| 层 | 技术 | 选型理由 |
|---|------|---------|
| **框架** | React 18 + TypeScript | 组件化天然适合仪表盘卡片模式；TypeScript 类型安全 |
| **构建** | Vite | 开发 HMR 极快，打包轻量 |
| **样式** | Tailwind CSS + 自定义主题 | 原子化 CSS 高效；自定义工业风 CSS 变量 |
| **状态管理** | Zustand + `persist` 中间件 | 轻量高性能，selector 精准重渲染，自动持久化 |
| **动画** | framer-motion | 进度条发光、数值跳动、机器运转指示灯动画 |
| **游戏引擎** | 自研纯函数引擎（`src/engine/`） | 纯逻辑无 I/O，在线 tick 与离线结算共用同一份定步长 tick 代码 |
| **大数** | `break_infinity.js`（Decimal） | 放置游戏数值远超 `Number.MAX_SAFE_INTEGER`；选浮点 Decimal 而非 BigInt——倍率/速率/`+4.2/s` 都是小数，BigInt 不能混算。引擎算式与存档序列化统一走 Decimal |
| **测试** | Vitest | 纯函数引擎是单测黄金场景：tick 推进、超频倍率、离线结算数值正确性 |
| **存储** | localStorage + IndexedDB | 双层存档防丢失 |
| **部署** | 静态托管（GitHub Pages / Vercel / Cloudflare Pages） | 零服务器成本 |

---

## 三、项目结构

```
greg-idle/
├── index.html
├── package.json
├── vite.config.ts
├── tailwind.config.ts
├── tsconfig.json
│
├── src/
│   ├── engine/                    # 游戏引擎（纯逻辑，无 I/O 依赖）
│   │   ├── tick.ts                # 每 tick 计算：机器产出、资源消耗、效率
│   │   ├── recipe.ts              # 配方匹配、输入验证、副产物产出
│   │   ├── era.ts                 # 石器/蒸汽/电力时代进度与解锁计算
│   │   ├── voltage.ts             # 电压过载/欠压/效率修正计算（LV 起）
│   │   ├── offline.ts             # 离线收益批量模拟结算
│   │   └── quest.ts               # 任务完成检测与奖励发放
│   │
│   ├── data/                      # 静态游戏数据（纯配置，不含逻辑）
│   │   ├── resources.ts           # 所有资源/材料定义（名称/形态/电压/图标）
│   │   ├── machines.ts            # 所有机器定义（名称/耗电/速度/配方槽）
│   │   ├── recipes.ts             # 所有加工配方（输入/输出/副产物/耗时/电压）
│   │   ├── quests.ts              # 任务书章节与任务节点定义（需求/奖励/依赖）
│   │   ├── era-roadmap.ts         # Stone→Steam→LV→...→UXV 总览路线
│   │   ├── voltage-tiers.ts       # 电压等级元数据（LV 起）
│   │   └── circuits.ts            # 电路等级定义（Primitive→Spacetime）
│   │
│   ├── stores/                    # Zustand 状态管理
│   │   ├── gameStore.ts           # 核心游戏状态（资源/机器/任务/tick）
│   │   ├── uiStore.ts             # UI 状态（当前面板/展开项/排序）
│   │   └── settingsStore.ts       # 设置（声音/通知/主题）
│   │
│   ├── components/                # React UI 组件
│   │   ├── layout/
│   │   │   ├── TopBar.tsx         # EU 电量仪表盘 + 电压等级标签 + 在线时长
│   │   │   ├── MainLayout.tsx     # 三栏响应式布局
│   │   │   └── BottomBar.tsx      # 事件日志流 + 警告徽章
│   │   ├── resource/
│   │   │   ├── ResourcePanel.tsx  # 资源仓库面板（左侧栏）
│   │   │   └── ResourceRow.tsx    # 单行资源：图标 + 名称 + 数量 + 变化速率
│   │   ├── workshop/
│   │   │   ├── WorkshopPanel.tsx  # 机器车间面板（中间栏）
│   │   │   ├── MachineCard.tsx    # ★ 核心组件：单台机器仪表盘卡片
│   │   │   └── RecipeSelector.tsx # 配方下拉选择器
│   │   ├── quest/
│   │   │   ├── QuestBookPanel.tsx # GTNH 风格任务书面板（右侧栏默认）
│   │   │   ├── QuestEraTabs.tsx   # Stone/Steam/LV/.../UXV 时代切换
│   │   │   ├── QuestNode.tsx      # 章节化任务图节点
│   │   │   └── EraRoadmap.tsx     # Stone→UXV 长线总览
│   │   └── common/
│   │       ├── ProgressBar.tsx    # 铜色渐变进度条
│   │       ├── PowerGauge.tsx     # EU 电量指针仪表盘
│   │       ├── VoltageBadge.tsx   # 电压等级发光标签
│   │       └── EraBadge.tsx       # 石器/蒸汽/电压时代标签
│   │
│   ├── hooks/
│   │   ├── useGameLoop.ts         # requestAnimationFrame 游戏主循环
│   │   ├── useOfflineCalc.ts      # 离线收益结算 hook
│   │   └── useSaveLoad.ts         # 存档管理 hook
│   │
│   ├── utils/
│   │   ├── storage.ts             # localStorage + IndexedDB 封装
│   │   ├── format.ts              # 大数格式化（K/M/B/T/Qa/Qi/Sx...）
│   │   └── id.ts                  # 唯一 ID 生成
│   │
│   ├── App.tsx
│   └── main.tsx
│
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-06-19-gtnh-idle-game-design.md  ← 本文档
```

---

## 四、核心 UI 布局

```
┌─────────────────────────────────────────────────────────────┐
│ ⚙ Era: HV  ⚡ EU: 12,847 / 20,480  ████████░░  ⏱ 4h32m     │  ← TopBar
├────────────────┬──────────────────────┬─────────────────────┤
│   资源仓库      │      机器车间         │      任务书          │
│   (LeftPanel)  │    (CenterPanel)     │    (RightPanel)     │
│                │                      │                     │
│ ┌────────────┐ │ ┌──────────────────┐ │ [Quests][Roadmap]  │
│ │ 🔩 铁锭     │ │ │ ⚡ 电力高炉       │ │  ┌─────────────┐  │
│ │   12,847    │ │ │ ████████░░  78% │ │  │ HV: Vacuum │✅│  │
│ │   +4.2/s   │ │ │ 铁锭 ×4         │ │  │ EBF Stable│✅│  │
│ │             │ │ │ [配方▼] [超频]   │ │  │ EV Gate   │⏳│  │
│ ├────────────┤ │ └──────────────────┘ │  └─────────────┘  │
│ │ 🔌 铜线     │ │                      │                    │
│ │   56,203    │ │ ┌──────────────────┐ │  Stone→...→UXV    │
│ │   -8.1/s   │ │ │ ⚡ 化学反应釜     │ │                    │
│ │             │ │ │ ████░░░░░░  35% │ │                    │
│ ├────────────┤ │ │ 橡胶条 ×8        │ │                    │
│ │ 🛢 橡胶条   │ │ │ [配方▼]          │ │                    │
│ │   3,241     │ │ └──────────────────┘ │                    │
│ │   +1.8/s   │ │                      │                    │
│ └────────────┘ │ ┌──────────────────┐ │                    │
│                │ │ ⚡ 离心机         │ │                    │
│ [形态筛选▾]   │ │ │ ░░░░░░░░░░  0% │ │                    │
│ [电压筛选▾]   │ │ │ [选择配方...]    │ │                    │
│ [🔍搜索]      │ │ └──────────────────┘ │                    │
│                │                      │                    │
├────────────────┴──────────────────────┴─────────────────────┤
│ [14:35:12] 电力高炉 → 铁锭 ×4               ⚠ 缺电！3台停机 │  ← BottomBar
└─────────────────────────────────────────────────────────────┘
```

右侧默认显示 Quest Book。`Roadmap` 只是总览 tab，用于展示 Stone→Steam→LV→...→UXV 的长期位置；玩家日常决策主要在资源仓库、机器车间和当前章节任务之间完成。

### MachineCard 组件细节（核心 UI 单元）

```
┌───────────────────────────────────────┐
│  ⚡ 电力高炉               [HV] ██ 85% │  ← 名称 + 电压标签 + 效率
│  ┌────────────────────────────────┐  │
│  │  配方: 铁锭                    │  │  ← RecipeSelector 下拉
│  │  ████████████████░░░░  78%    │  │  ← 铜色渐变进度条
│  │  输入: 铁矿×2   输出: 铁锭×4   │  │  ← 输入/产出
│  │  副产: 金粉×1   耗时: 12.8s   │  │  ← 副产物（GTNH 味！）
│  └────────────────────────────────┘  │
│  [⚡ 超频 +50%] [🔧 升级] [⏹ 停机]  │  ← 操作按钮
└───────────────────────────────────────┘
```

### Quest Book 与 Roadmap

面向熟悉 GTNH 的玩家，主引导不是抽象科技树，而是 GTNH 风格任务书。

- **Quest Book**：回答“下一步做什么”。默认显示在右侧面板，按 Stone Age、Steam Age、LV、MV、HV、EV、IV、LuV、ZPM、UV、UHV、UEV、UIV、UMV、UXV 分时代页展示。
- **时代任务图**：每个 Era 内部是一张小型 DAG，节点包含任务名、描述、需求、奖励、依赖和“定位相关机器/资源”按钮。
- **Roadmap**：回答“我现在在哪、长期目标到哪”。它横向展示 Stone→Steam→LV→...→UXV，只做总览和跳转，不承担具体操作。
- **设计原则**：玩家不是消耗研究点“点科技”，而是真的把那条 GTNH 生产链做出来。任务奖励主要解锁机器、配方、自动化能力、QoL 和小倍率。

术语约定：代码与数据模型统一使用 `Era`。文案中的“章节”只指 Quest Book 的表现层，不作为独立领域概念。

任务节点类型：

| 类型 | 用途 | 示例 |
|---|---|---|
| `Milestone` | 时代关键节点 | 进入 Steam Age、获得第一台 LV 机器 |
| `Craft` | 持有或累计生产某物 | Flint Tool、Bronze Plate、Basic Circuit |
| `Automation` | 达到持续产率 | 木头 +1/s、蒸汽 32 L/s、铁锭 +0.2/s |
| `Machine` | 建造机器或运行配方 | Primitive Furnace、Steam Macerator、Electric Furnace |
| `Choice` | 允许熟人玩家选择路线 | 先做燃料线、矿物处理线、橡胶线 |
| `Challenge` | 可选硬核目标 | 低耗电运行、无停机持续生产 30 分钟 |

#### CSS 主题变量

```css
:root {
  --color-bg-primary: #1a1a2e;        /* 深灰蓝底色 */
  --color-bg-panel: #252540;          /* 面板底色 */
  --color-bg-card: #2a2a45;           /* 卡片底色 */
  --color-border: #4a4a6a;            /* 边框 */
  --color-accent: #f0a500;            /* 暖黄主色调 */
  --color-accent-glow: #f5c842;       /* 发光暖黄 */
  --color-progress: #c47f30;          /* 铜色进度条 */
  --color-progress-glow: #e8a850;     /* 进度条发光 */
  --color-text-primary: #e0e0e0;      /* 主文字 */
  --color-text-secondary: #a0a0b0;    /* 次要文字 */
  --color-danger: #e05555;            /* 警告/缺电红 */
  --color-success: #55c055;           /* 完成绿 */
  --font-mono: 'JetBrains Mono', 'Consolas', monospace; /* 读数等宽 */
}
```

---

## 五、核心数据模型

### 5.1 时代与电压

```typescript
type Era =
  | 'stone'
  | 'steam'
  | VoltageTier;
```

石器与蒸汽时代没有 EU 电压，但它们和 LV 以后的电压时代在 Quest Book / Roadmap 上属于同一条长线进度。引擎层要允许不同能量机制共存：手动动作、燃料燃烧、蒸汽、EU。

### 5.2 电压等级

电压从 LV 开始，Roadmap 终点为 UXV。MAX 可作为远期隐藏终局或重开入口，但不进入第一阶段设计。

数值取自 GT5-Unofficial 源码 `GTValues.V`（1A 单包 EU/t 上限）与 `GTValues.VP`（配方实用值 = V × 30/32）。

```typescript
type VoltageTier =
  | 'LV'   // 32 EU/t      (VP 30)     — 低压
  | 'MV'   // 128 EU/t     (VP 120)    — 中压
  | 'HV'   // 512 EU/t     (VP 480)    — 高压
  | 'EV'   // 2,048 EU/t   (VP 1,920)  — 超高压
  | 'IV'   // 8,192 EU/t   (VP 7,680)  — 精英
  | 'LuV'  // 32,768 EU/t  (VP 30,720) — 发光
  | 'ZPM'  // 131,072 EU/t (VP 122,880)— 零點
  | 'UV'   // 524,288 EU/t (VP 491,520)— 极限
  | 'UHV'  // 2,097,152 EU/t           — 究极高压
  | 'UEV'  // 8,388,608 EU/t           — 究极极限
  | 'UIV'  // 33,554,432 EU/t          — 究极精英
  | 'UMV'  // 134,217,728 EU/t         — 究极中压
  | 'UXV'; // 536,870,912 EU/t         — 究极极限高压
```

> 电压等级每级为上一级的 4 倍。源码还定义了 ULV（8 EU/t）和 MAX（2,147,483,640 EU/t），GregIdle 将它们保留为内部边界和远期终局入口。UHV 及以上不存在 VP 值——VP 是 GT5U 实现细节，仅存在于 LV–UV 范围，此后配方直接使用 V 值。

### 5.3 资源与材料

```typescript
type MaterialForm =
  | 'ore'      // 矿石
  | 'crushed'  // 粉碎矿
  | 'purified' // 纯净矿
  | 'dust'     // 粉尘
  | 'ingot'    // 锭
  | 'plate'    // 板
  | 'rod'      // 杆
  | 'wire'     // 线
  | 'fluid'    // 流体
  | 'gas'      // 气体
  | 'plasma';  // 等离子体

interface Resource {
  id: string;              // 唯一标识 'iron_ingot'
  name: string;            // 显示名称 '铁锭'
  form: MaterialForm;      // 材料形态
  era: Era;                // 首次出现时代
  baseValue: number;       // 基础经济价值
  fluid: boolean;          // 是否流体（影响存储/运输）
  icon: string;            // 图标标识
  description?: string;    // 描述
}
```

### 5.4 机器

```typescript
type MachineRole =
  | 'manual_station'    // 手动/半手动作业台，如 Workbench、Mortar
  | 'passive_source'    // 自动采集点，允许 recipe inputs 为空
  | 'processor'         // 普通加工机器
  | 'energy_producer'   // 能源生产，如 Boiler、Generator
  | 'energy_converter'  // 能源转换，如 Steam Dynamo: steam -> EU
  | 'storage';          // 缓冲/电池/蒸汽罐

interface MachineDef {
  id: string;              // 'electric_blast_furnace'
  name: string;            // '电力高炉'
  era: Era;                // 首次出现时代
  role: MachineRole;       // 决定 tick 处理方式
  tier?: VoltageTier;      // 最低运行电压（蒸汽/石器机器为空）
  euPerTick?: number;      // 基础耗电 (EU/t)
  steamPerSecond?: number; // 蒸汽机器消耗
  fuelPerSecond?: number;  // 锅炉/燃烧类设施消耗
  produces?: Partial<EnergyDelta>; // 能源生产或转换后的净产出
  baseSpeed: number;       // 基础加工时间 (ticks)
  recipeSlots: number;     // 可同时运行的配方数
  overclockable: boolean;  // 是否支持超频
  upgradeable: boolean;    // 是否可升级
  description: string;
}

interface PlayerMachine {
  defId: string;           // 对应 MachineDef.id
  instanceId: string;      // 实例唯一 ID
  tier?: VoltageTier;      // 当前供电压等级（电力机器）
  overclockLevel: number;  // 超频等级
  upgradeLevel: number;    // 升级等级
  selectedRecipeId?: string; // 持久态：玩家已选配方。停机后仍保留，复机/读档不丢选择
  active: boolean;         // 是否运行中
}
```

Stone Pit / Wood Lot / Clay Deposit 不引入独立 `PassiveGenerator` 类型，统一建模为 `MachineDef.role = 'passive_source'`，并使用 `inputs: {}` 的配方表示净产出资源。锅炉、发电机、蒸汽转 EU 设备也统一是 `MachineDef`，通过 `role` 和能源字段区分 tick 行为。

### 5.5 配方

```typescript
interface Recipe {
  id: string;                    // 'iron_ingot_from_ore'
  machineId: string;             // 所需机器类型
  name: string;                  // '烧制铁锭'
  inputs: Record<string, number>;  // { 'iron_ore': 2 }
  outputs: Record<string, number>; // { 'iron_ingot': 4 }
  byproducts: Record<string, number>; // { 'gold_dust': 1 }  ← GTNH 核心特色！
  baseDuration: number;          // 基础加工 tick
  era: Era;                      // 配方所属时代
  tier?: VoltageTier;            // 配方电压等级（LV 起）
  minMachineTier?: VoltageTier;  // 最低机器要求
  minCircuitTier?: number;       // 最低电路等级门槛（对应 7.5，由 raiseCircuitTier 奖励抬升后解锁）
  euPerTick?: number;            // 额外耗电（覆盖机器默认）
  steamPerSecond?: number;       // 蒸汽配方消耗
}
```

### 5.6 运行中配方

```typescript
interface ActiveRecipe {
  machineInstanceId: string;
  recipeId: string;
  progress: number;        // 0-1
  elapsedMs: number;
  pausedReason?: 'missing_input' | 'no_energy' | 'storage_full' | 'disabled';
}
```

### 5.7 任务书

```typescript
type QuestNodeType =
  | 'Milestone'
  | 'Craft'
  | 'Automation'
  | 'Machine'
  | 'Choice'
  | 'Challenge';

interface QuestNode {
  id: string;
  era: Era;
  type: QuestNodeType;
  title: string;
  description: string;
  requirements: QuestRequirement[];
  rewards: QuestReward[];
  prerequisites: string[];
  optional: boolean;
}

type QuestRequirement =
  | { type: 'resourceHeld'; resourceId: string; amount: number }
  | { type: 'resourceProduced'; resourceId: string; amount: number }
  | { type: 'machineBuilt'; machineId: string; count: number }
  | { type: 'recipeCompleted'; recipeId: string; count: number }
  | { type: 'rateReached'; resourceId: string; perSecond: number }
  | { type: 'energyStored'; energy: 'steam' | 'eu'; amount: number }
  | { type: 'questCompleted'; questId: string };

type QuestReward =
  | { type: 'unlockRecipe'; recipeId: string }
  | { type: 'unlockMachine'; machineId: string }
  | { type: 'unlockEra'; era: Era }
  | { type: 'grantResource'; resourceId: string; amount: number }
  | { type: 'unlockAutomation'; automationId: string }
  | { type: 'modifyRate'; targetId: string; multiplier: number }
  | { type: 'raiseCircuitTier'; tier: number }  // 抬升 PlayerState.circuitTier，兑现 7.5 电路门控
  | { type: 'unlockQoL'; featureId: string };
```

> `raiseCircuitTier` 是 7.5 节「电路等级作为解锁门槛」在数据模型里的落点：任务完成后把 `PlayerState.circuitTier` 抬到指定等级，后续配方/机器的 `minCircuitTier` 检查才能通过。没有这一项，电路门控在引擎里无法兑现。

### 5.8 能源状态

```typescript
interface EnergyState {
  steamStored: number;
  steamCapacity: number;
  euStored: number;
  euCapacity: number;
}

interface EnergyDelta {
  steam: number;
  eu: number;
}
```

`steamStored` 是蒸汽时代的关键缓冲：锅炉消耗燃料产生蒸汽，蒸汽机器从缓冲中取用。`euStored` 是 LV 起 TopBar 的主要读数：发电机/转换器产出 EU，电力机器消耗 EU。

**蒸汽 → EU 换算**：GTNH 中蒸汽机器按 `2 L steam = 1 EU` 的比例展示消耗。Steam Dynamo 等桥接设备内部使用此比例将蒸汽转化为 EU。这一换算比是 GregIdle 蒸汽时代与电力时代之间能源转换的基础数值。出处：GTNH Steam Age Wiki。

### 5.9 玩家状态

```typescript
interface PlayerState {
  // 资源
  resources: Record<string, number>;

  // 能源
  energy: EnergyState;
  
  // 机器
  machines: PlayerMachine[];
  activeRecipes: ActiveRecipe[];
  
  // 时代与任务
  currentEra: Era;
  circuitTier?: number;           // 当前最高电路等级 (0-8+)，影响可解锁配方/机器
  completedQuests: string[];
  unlockedEras: Era[];
  
  // 统计
  totalProduction: number;
  totalPlayTime: number;          // 毫秒
  lastTickTime: number;           // 最后 tick 时间戳
  createdAt: number;
}
```

---

## 六、游戏核心循环

### 6.1 主 Tick 循环

```
每帧 (requestAnimationFrame, ~16ms):

1. 计算 deltaTime = now - lastTickTime
2. 能源计算:
   ├── Stone: 处理手动动作冷却、基础设施产出
   ├── Steam: 锅炉消耗燃料 → 增加 steamStored，蒸汽机器消耗 steamStored
   └── LV 起: 发电/转换设备增加 euStored，电力机器消耗 euStored
3. 设施与机器遍历:
   ├── passive_source: 按空输入配方直接产出资源
   ├── energy_producer / energy_converter: 处理燃料、蒸汽和 EU 净变化
   ├── processor: 按 (deltaTime / recipeDuration × efficiency) 推进进度
   ├── 进度满 → 消耗输入、生成输出 + 副产物
   └── 输入不足、能源不足或存储满时写入 pausedReason
4. 任务与时代检测:
   ├── 检查 QuestNode requirements 是否满足
   ├── 发放任务奖励（配方、机器、自动化能力、QoL、倍率）
   └── 满足里程碑时解锁下一时代章节
5. 更新 UI (通过 Zustand selector 触发重渲染)
```

### 6.2 离线结算

```
玩家重新打开页面:

1. 读取 lastTickTime
2. offlineDuration = now - lastTickTime（先经 6.4 防作弊校验）
3. 调用 engine/offline.ts → calculateOffline(state, offlineDuration)
   - 复用在线那份纯函数 tick，但用粗粒度定步长（默认 1s/步，离线越久步长越大）
   - 每一大步跳过 UI 事件与动画，只推进资源/能源/进度状态
   - 累加每步的产出/消耗，最后一次性写回，避免逐 16ms 帧重算
4. 应用结果到状态
5. 显示离线收益摘要弹窗（产出、消耗、因断料/断电停机的时长）
```

> **算法决策**：离线结算与在线 tick **共用同一份纯函数**（见技术选型「游戏引擎」行），仅步长不同——在线 ~16ms/帧，离线按时长放大到 1s~分钟级。不采用「直接解最大产量」的闭式公式：产线存在机器互相喂料、缓冲上限与副产物，一旦出现依赖回路就退化为流量/线性规划问题，没有简单解，且会与在线数值产生肉眼可见的偏差。定步长模拟是「能共用代码 + 数值自洽」的唯一稳妥选择；「按瓶颈直接求产量」只作为后期纯线性产线的可选性能优化，不作为主路径。

### 6.3 电压效率机制

```typescript
interface VoltageEffect {
  efficiency: number;
  euMultiplier: number;
}

function getVoltageEffect(machineTier: VoltageTier, supplyTier: VoltageTier): VoltageEffect {
  const tierDiff = getTierIndex(supplyTier) - getTierIndex(machineTier);
  if (tierDiff < 0) return { efficiency: 0, euMultiplier: 0 }; // 供电不足，无法运行
  if (tierDiff === 0) return { efficiency: 1.0, euMultiplier: 1.0 };
  if (tierDiff === 1) return { efficiency: 2.0, euMultiplier: 4.0 };
  return { efficiency: 2.0, euMultiplier: 8.0 }; // 封顶：效率锁 2×，耗电从 4× 抬到 8×后不再涨，防早期机器后期无限滚雪球
}
```

设计意图：高一级供电提供 2×效率，但消耗 4× EU；高两级及以上不继续提高效率，只进一步提高耗电，避免早期机器在后期无限滚雪球。

> **与真实 GT5U 的关系**：GT5U 的 `OverclockCalculator` 标准行为是每次 overclock EU×4、耗时÷2，部分机器有自定义倍率（如 EBF 的热量完美超频），多方块允许安培参与超频。GregIdle 的 `getVoltageEffect()` 是放置游戏简化版——不去逐级计算 overclock 次数，直接用供电等级差映射效率和耗电倍率，保留"高电压=高效但费电"的核心体验。

---

## 七、时代进阶系统（GTNH 灵魂）

> 本节数据对照 GTNH 2.9.0-beta-1 游戏内任务书（46 条 QuestLine、3749 个任务节点）与 Dev-Doc 16 阶段划分。
> GregIdle 将其抽象为放置节奏——真实 GTNH 中 Stone Age 有 92 个任务（38 主线），在 GregIdle 中压缩为 6-8 个关键任务节点。
>
> **GTNH 时代设计纲领（摘自 Dev-Doc）**：
> - 每个 Tier 引入新机器、新工艺和新维度/星球，用新材料解锁下一级
> - 某些材料在早期 tier 稀缺（如 Titanium），随后期 tier 逐渐变得更容易获得（通常需建完整产线）
> - 电路 tier 与电压 tier 不同步：LV 机器用 MV 电路、HV 时代即制作 IV 电路——这是 GTNH 体验的核心
> - Tier 材料用于门控内容（如用 Titanium Plate 限制 Thaumcraft 物品）

### 7.1 节奏设计

> 以下数据已与 GTNH Dev-Doc 各阶段的主材料、线缆材料、管道材料、主要挑战和 End of Tier 条件对照验证。
> 产出倍率（1×/2×/4×/8×/16×/32×/64×/128×）反映 GTNH 中电压对应的理论速率系数，实际游戏内通过 `getVoltageEffect()` 简化映射。
>
> ⚠️ **时间标注是「原作参考」，不是 GregIdle 目标**：下方每个时代后面的时长（Stone 0-10 分钟 … UV 4-6 个月）是真实 GTNH 通关者的**日历耗时**，仅用于体现各阶段的相对体量，**不是 GregIdle 的目标节奏**。GregIdle 作为放置翻译，自己的节奏曲线见 7.1.1。

#### 7.1.1 GregIdle 目标节奏曲线

GregIdle 的时间锚是**有效游玩时间**（在线 + 离线结算累计的活跃推进时长，不含纯停滞挂机的空转），不是日历时间。目标是把真实 GTNH 数百到上千小时压缩到放置游戏可接受的尺度——**到 UV 约 1-2 周有效时间**。

| Era | 原作日历参考 | GregIdle 目标（有效时间，累计） | 说明 |
|---|---|---|---|
| Stone | 0-10 分钟 | ~10 分钟 | 与第一版开局节奏（7.3）一致 |
| Steam | 10 分钟-1 小时 | ~1 小时 | 产线规划起步 |
| LV | 1-4 小时 | ~半天 | 第一版终点 |
| MV | 4 小时-2 天 | ~1 天 | |
| HV | 2 天-1 周 | ~2-3 天 | 化学链 + 多方块前置密度上来 |
| EV | 1 周-1 月 | ~4-5 天 | AE2/批量自动化转折 |
| IV | 1-2 月 | ~1 周 | |
| **LuV→UV** | 2-6 月 | **累计 1-2 周** | 后期靠自动化与离线结算压缩 |
| UHV→UXV | 远期 | 远期持续迭代 | Phase 3+，不锚定时长 |

> 这条曲线是**平衡调优的锚**，不是承诺值。Phase 1 只验证到 LV（~半天有效时间）。后期每扩一个时代，按此曲线校准配方耗时、产率倍率与解锁门槛；离线结算（6.2）是把后期日历跨度压进「有效时间」预算的关键手段。具体数值在数据落地后用模拟器（见风险表）回归调整。

```
Stone Age   ████░░░░░░  0-10 分钟
├─ 解锁: 手动采集、Flint Tool、Mortar、Primitive Furnace
│         Tinkers' 工具体系、Coke Oven（焦炉自动化 → 煤焦/杂酚油）
│         Macerator v0.1 / Forming Press v0.1（手动加工雏形）
│         基础存储: Drawers / Barrels / 铁箱
│         Ore Finder Wand（手动找矿辅助）
├─ 材料: Flint（工具）、Iron（后期工具）、木头、石头、燧石、黏土
│         （GTNH Dev-Doc: Flint 为主材料，Iron 为次要工具材料）
├─ 痛点: 手动动作多、产率低、矿脉识别（GT 矿脉生成机制）
├─ 里程碑: Small Coal Boiler（首台锅炉，Stone→Steam 门票）
│         └─ GTNH Dev-Doc: Stone 结束条件是做出 Small Coal Boiler
├─ 自动化奖励: Stone Pit / Wood Lot / Clay Deposit（对应真实 GTNH 中基础资源采集自动化）
│
├─► Steam Age  ██████░░░░  10 分钟 - 1 小时
│   解锁: 机器三件套（Machine Hulls + Motors + Rotors）
│         Steam Macerator、Steam Compressor、Steam Forge Hammer
│         Bricked Blast Furnace（BBF）、第一块电子电路
│         Forestry Worktable（批量合成辅助）
│         Steam Grinder 多方块（125% 速度 / 62.5% 蒸汽消耗 / 8 并行）
│   材料: 青铜（机器外壳）、Wrought Iron（熟铁）
│         管道: 青铜、锡
│         后期: Steel（钢，BBF 产出，用于更高阶工具）
│         线缆: 无（Steam 时代无电力）
│   痛点: 稳定蒸汽供给（断汽拖慢全产线）、BBF 钢线节奏
│   参考: Steam Grinder 多方块（125% 速度 / 62.5% 蒸汽消耗 / 8 并行）可作为中期升级的数据参照
│   自动化奖励: 蒸汽机器队列、基础矿物处理（粉碎/研磨增产）、Hang Glider 移动辅助
│   └─ GTNH Dev-Doc: Steam 结束条件是 LV circuits + 第一台电力机器
│
├─► LV (32 EU/t)  ████████░░  1 小时 - 4 小时
│   解锁: Bending Machine、Wiremill、Assembler、Electrolyzer、Chemical Reactor
│         电热高炉 EBF ★（LV→MV 软卡点）
│         化学链起步: Chlorine、Hydrogen、酸碱
│         初期自动化: Conveyor / Pump 覆盖板、LV 机器自动输出
│         首台自动采矿机（弱）+ 探测器扫描仪
│   材料: Steel（主）、Wrought Iron（次）
│         线缆: Tin、Copper
│         管道: Bronze、Tin、Potin
│   痛点: 钢材短缺（EBF 与电力机器持续消耗钢）
│         供电转型: Dev-Doc 建议 EBF 前脱离纯蒸汽
│         Steam Turbine 效率低 → 可能导致 EBF 断电损毁原料
│   产出: 1×
│   └─ GTNH Dev-Doc: LV 结束条件是 MV circuits + 第一台 MV 机器
│
├─► MV (128 EU/t) ████████░░  4 小时 - 2 天
│   解锁: Advanced Circuits / Solar Grade Silicon（首个化学线）
│         MV Electrolyzer（大量产 Alumina Dust）/ Extruder
│         Kanthal EBF 线圈升级、Large Chemical Reactor
│         EnderIO Conduits（辅助自动化）
│         Coal Jetpack → Copterpack（移动升级）
│   材料: Aluminium（主）、Wrought Iron（次）
│         线缆: Copper、Cupronickel
│         管道: Steel、Potin、Dark Steel
│   痛点: EBF 长时间稳定运行、Kanthal 升级
│         电力需求跃升——需要真正的电力基础设施（Oil / Benzene / Charcoal→Steam）
│   产出: 2×  （过载惩罚: 4× 耗电）
│   └─ GTNH Dev-Doc: MV 结束条件是 HV circuits + 第一台 HV 机器
│
├─► HV (512 EU/t) ██████████  2 天 - 1 周
│   解锁: Cleanroom ★（门控所有新电路——先建无尘室才能做新电路）
│         Vacuum Freezer、Implosion Compressor
│         Large Chemical Reactor / Distillation Tower / Oil Cracker
│         Simple SoC（片上系统）
│         Tier 1 Rocket（NASA Workbench → 登月 → Titanium）
│         LCR 无电池化工（cell-free chemical processing）
│         Nano Armor、Drawer Controller、Industrial Apiary
│   材料: Stainless Steel（主）、Polyethylene（次）
│         线缆: Gold、Silver、Electrum
│         管道: Stainless Steel、Dark Steel
│         火箭: Tier 1 — 月球
│   痛点: Cleanroom 结构挑战（门控高阶电路与配方环境）
│         NASA Workbench + T1 Rocket 是 AE2 前最复杂的工艺
│   产出: 4×
│   └─ GTNH Dev-Doc: HV 结束条件是获得足够 Titanium 准备 EV 机器
│
├─► EV (2,048 EU/t) ████████████  1 周 - 1 月
│   解锁: AE2 ★（ME 存储与自动化——GTNH "最难整合包"的核心转折点）
│         GT++ 多方块（包括矿石处理多方块）
│         Nanocircuits、Tier 2 Rocket（→ Mars / Deimos / Phobos）
│         Platinum/Radon（Thorium）/ Tungstensteel 产线
│         必需化工: Epoxid、Polyphenylene Sulfide、Graphene
│         LSC（Lapotronic Supercapacitor）末期
│         LV SoC、Ore Drilling Plant、Processing Array（末期）
│   材料: Titanium（主）、Polyethylene（次）
│         线缆: Aluminium、Black Steel
│         管道: Titanium
│         火箭: Tier 2 — 火星 / Deimos / Phobos
│   痛点: 从单方块发电机转向多方块发电机、从分散电力到集中电力
│         AE2 自动化初建（Titanium 与电力成本是主要障碍）
│   产出: 8×
│   └─ GTNH Dev-Doc: EV 结束条件是获得一叠 Tungstensteel
│
├─► IV (8,192 EU/t) ██████████████  1 月 - 2 月
│   解锁: Assembly Line ★（GTNH 中 EV→IV 最重磅建造项目）
│         更多 GT++ 多方块、Processing Array 大规模并联
│         铂线完整自动化 ★（Rhodium、Palladium、Osmium、Iridium、Ruthenium）
│         PBI（Polybenzimidazole）、Indium 线、Monazite/Bastnasite 线
│         Quantumcircuits、Tier 3+4 火箭
│         QuantumSuit、Vajra、AE2 更多自动化组件
│   材料: Tungstensteel（主）、PTFE（次）
│         线缆: Tungsten、Graphene、Platinum
│         管道: Tungstensteel
│         火箭: Tier 3 — Ceres / Asteroids / Europa / Ganymede / Callisto / Ross128b
│         Tier 4（LuV 交界）— Io / Mercury / Venus
│   痛点: 几十个多方块并行运行才能保证产率
│         铂线是全包最复杂化学链（6 种主产物全用于后续）
│         Tungsten + Tungstensteel 在 EBF 中极长时间处理
│         大量 Graphene 需求 → 巨量 Silicon 处理
│   产出: 16×
│   └─ GTNH Dev-Doc: IV 结束条件是建成 Assembly Line + 第一台 LuV 机器
│
├─► LuV (32,768 EU/t) ██████████  2 月 - 3 月
│   解锁: Naquadah 线起始 ★、第一台 Fusion Reactor
│         CALs（Circuit Assembly Lines，大幅加快电路生产）
│         Precise Assembler、Auto Maintenance Hatch
│         Crystalcircuits、Crafting Input Bus
│   材料: Rhodium-Plated Palladium（主）、PTFE（次）
│         线缆: Vanadium-Gallium、Yttrium Barium Cuprate
│         管道: Niobium-Titanium
│         火箭: Tier 5 — Enceladus / Titan / Miranda / Oberon / Ross128ba
│   痛点: 聚变第一代建造（大量冶炼→大量电力需求）
│         Assembly Line 完全自动化
│   产出: 32×
│
├─► ZPM (131,072 EU/t) ██████  3 月 - 4 月
│   解锁: Naquadah 线完成（Enriched Naquadah / Naquadria）
│         Mk II Fusion Reactor
│         首台 Void Miner、Data Bank
│   材料: Iridium（主）、PBI（次）
│         线缆: Naquadah、Vanadium-Gallium、Europium
│         管道: Enderium
│         火箭: Tier 6 — Oberon / Triton
│   痛点: 为 Neutronium 升级足够的冶炼和电力能力
│   产出: 64×
│
├─► UV (524,288 EU/t) ████████  4 月 - 6 月
│   解锁: 生物工艺（Biolab / Biovat）、TecTech 计算与研究
│         Mk III Fusion、PCB Factory
│         Lasers / Active Transformer
│         Wetware Circuits、Research Station、Quantum Computer
│   材料: Osmium（主）、PBI（次）
│         线缆: Naquadah Alloy、Americium、Fluxed Electrum
│         管道: Naquadah
│         火箭: Tier 7 — Pluto / Kuiper Belt / Haumea / Makemake
│   可选: Space Elevator (末期/UHV)、Compact Fusion (UHV)
│   产出: 128×
│
├─► UHV → UEV → UIV → UMV → UXV
│   UHV: 电能激增、DTPF 多方块、Spacetime 材料
│   UEV-UMV: 极端能量级别、奇点级目标
│   UXV: 536,870,912 EU/t——GTNH 当前设计远端
│
└─► 远期终局 / 可选 Prestige
    UXV 后可考虑奇点、宇宙重启或隐藏 MAX，但不进入早期实现范围

每个时代都要引入一种新问题，而不是只把数字乘大。下表已与 GTNH Dev-Doc 各阶段的主材料、主要挑战和 End of Tier 条件对照验证。

| 阶段 | GTNH 真实痛点 | GregIdle 主问题 | 自动化奖励 |
|---|---|---|---|
| Stone | 生存压力 + 工具与找矿压力（GT 矿脉网格识别） | 手动动作与基础材料短缺 | 自动采集点、工作台队列 |
| Steam | 稳定蒸汽供给 + BBF 钢线节奏 | 燃料与蒸汽压力 | 蒸汽机器队列、基础矿物处理、Ore doubling（粉碎→研磨增产） |
| LV | 钢材短缺 + 供电转型（蒸汽→EU）+ EBF 软墙（Steam Turbine 效率低可致断电损毁原料） | EU供电、电缆、电路门槛 | 电力机器、配方自动运行、初期自动化覆盖板（Conveyor/Pump） |
| MV | EBF 长时间运行 + Kanthal 线圈升级 + 电力需求跃升需真正电力基础设施 | 进阶电路与合金产线、首条化学线（Solar Grade Silicon） | 多机器串联、基础副产物管理、化学产线雏形 |
| HV | Cleanroom 结构挑战（门控所有新电路）+ 登月与钛 | 化学链、多方块与航天前置、PTFE + IV 电路 | 多方块链路（LCR / Distillation Tower / Vacuum Freezer）、副产物系统 |
| EV | AE2 初建 + 从单方块到多方块发电 + Platinum 线 | AE/批量队列/高阶自动化、Tungstensteel | 大规模并行、智能物流、LSC 能源缓冲 |
| IV | 铂线全自动化（6 种产物）+ Assembly Line + 巨量化工 | 稀有中间件与网络化生产、数十个多方块并行 | GT++ 多方块阵列、Processing Array 批量 |
| LuV | Naquadah 线起始 + 第一台聚变 + 全自动 Assembly Line | 聚变能量、CALs、Crystalcircuits | 终极电路生产、Auto Maintenance Hatch |
| ZPM-UV | Naquadah 线完成 + 生物工艺 + TecTech 计算 | 大规模并行、奇点级目标 | Void Miner、PCB Factory、Wetware  |
| UHV-UXV | 极端吞吐和终局材料 | 大规模并行、奇点级目标 | 终极自动化 |

### 7.2 时代解锁条件示例

对照 GTNH Dev-Doc 各阶段的 End of tier 条件。GregIdle 中所有条件均为持有/累计/进度检查，不要求实时战斗、空间探索或多方块搭建。

```
Stone → Steam: Small Coal Boiler ×1 + Charcoal ×32 + Bronze Plate ×16
               (GTNH Dev-Doc: Stone 结束条件是做出 Small Coal Boiler)
Steam → LV:   Basic Steam Dynamo ×1 + LV Machine Hull ×1 + Tin Cable ×8 + LV Circuits ×4
               (GTNH Dev-Doc: Steam 结束条件是 LV circuits + 第一台电力机器)
LV → MV:      Assembler ×1 + EBF 稳定运行 + 基础电路板 ×16 + 橡胶 ×32 + MV Circuits ×8
               (GTNH Dev-Doc: LV 结束条件是 MV circuits + 第一台 MV 机器)
MV → HV:      Data Orb ×4 + Kanthal EBF 线圈升级 + 高级电路板 ×32 + Stainless Steel Ingot ×64 + HV Circuits ×8
               (GTNH Dev-Doc: MV 结束条件是 HV circuits + 第一台 HV 机器)
HV → EV:      Processor Assembly ×1 + Titanium Plate ×128 + Cleanroom 建成 + IV Circuits ×4
               (GTNH Dev-Doc: HV 结束条件是获得足够 Titanium 准备 EV 机器)
EV → IV:      Tungstensteel Ingot ×64 + Nanocircuits ×16 + T2 Rocket 完成
               (GTNH Dev-Doc: EV 结束条件是获得一叠 Tungstensteel)
IV → LuV:     Assembly Line ×1 + Quantumcircuits ×8 + LuV Machine Hull ×1
               (GTNH Dev-Doc: IV 结束条件是建成 Assembly Line + 第一台 LuV 机器)
LuV → ZPM:    Fusion Reactor Mk I + Crystalcircuits ×8 + Europium Ingot ×32
               (GTNH Dev-Doc: LuV 结束条件是 Europium + 第一台 ZPM 机器)
ZPM → UV:     Fusion Reactor Mk II + Americium ×16 + Naquadria ×64
               (GTNH Dev-Doc: ZPM 结束条件是 Americium + Naquadria + 第一台 UV 部件)
UV → UHV:     Wetware Circuits + Cosmic Neutronium + Bedrockium + UHV Circuits
               (GTNH Dev-Doc: UV 结束条件是 Cosmic Neutronium + Bedrockium + UHV circuits)
...
```

`Basic Steam Dynamo` 是 Steam Age 末期的桥接设施：消耗蒸汽、低效率产出 EU。它不是 LV 机器本体，也不需要 LV 供电，因此不会形成“需要 LV 才能解锁 LV”的循环依赖。第一台真正的 LV 机器应在该桥接完成后制造。

### 7.3 第一版开局节奏（C 型折中）

第一版从 Stone Age 开始，但不把手动采集拖太久。目标是在前 5 分钟让熟悉 GTNH 的玩家感到“这就是起步痛点”，随后立刻给自动化作为奖励。

| 时间 | 玩家行为 | 设计目的 |
|---|---|---|
| 0-1 分钟 | 手动采集木头、石头、燧石 | 建立原始开局感 |
| 1-3 分钟 | 制作 Flint Tool、Mortar、Workbench | 引入基础合成链 |
| 3-5 分钟 | 收集黏土/木炭前置，完成第一个采集类任务 | 让手动操作达到“有点烦但还没厌烦”的边界 |
| 5-10 分钟 | 解锁 Stone Pit / Wood Lot / Clay Deposit | 用自动采集点消灭第一个痛点 |
| 10-30 分钟 | 进入青铜与蒸汽前置，准备 Bronze Boiler | 把目标从点击采集转向产线规划 |
| 30-90 分钟 | 蒸汽机器推进到 LV 入门 | 第一版终点：造出第一台 LV 电力机器 |

早期设计原则：每个时代先让玩家短暂体验痛点，再通过任务奖励给出自动化能力。手动动作只用于建立 GTNH 味道，不应成为长期重复劳动。

#### 7.3.1 中期活跃玩法循环（Steam→HV）

7.3 只覆盖了前 90 分钟。解锁自动采集点后、Prestige 之前（Prestige 推到远期），玩家「每分钟在做什么」靠下面这个循环支撑——放置游戏中段最容易退化成纯等待，这里明确玩家的主动决策点：

```
观察 → 决策 → 投入 → 等待结算 → 再观察
```

| 决策点 | 玩家动作 | 触发频率 |
|---|---|---|
| **解瓶颈** | BottomBar 报「缺料/缺电/存储满」→ 定位短缺资源 → 加产线或加缓冲 | 每次产线扩张后高频 |
| **选配方** | 同一台机器多配方可选（如离心机），按当前任务目标切换 | 每个新任务节点 |
| **建/升机器** | 任务奖励解锁新机器或升级 → 决定建几台、配什么电压 | 每个 Milestone |
| **供电规划** | 发电类型随时代切换（Steam→Oil→Nuclear→Fusion），平衡产电与耗电 | 每个时代转换 |
| **路线选择** | `Choice` 类任务：先做燃料线 / 矿物线 / 橡胶线 | 关键岔路 |
| **超频取舍** | 高一级供电 2× 效率但 4× 耗电（6.3）——值不值得 | 电力充裕时 |

设计原则：玩家不该盯着进度条空等。每一步「等待」后面都应紧跟一个「值得回来做的决策」——通常是解一个新瓶颈或选下一条产线。离线结算（6.2）负责吸收长时间空窗，让玩家回来时面对的是「攒够了料，该扩产了」而不是「白等了一段」。纯挂机只在玩家主动选择放养时发生，不是默认玩法。

### 7.4 矿石增产链抽象（GTNH 多方块碾压模型）

GTNH 的矿石处理是一套多阶段增产链。GregIdle 将其抽象为机器升级或多方块搭建立即生效的倍率，而非逐一追踪粉碎→清洗→离心→研磨流程。

| GTNH 阶段 | GTNH 机器 | 累计产出 | GregIdle 抽象 |
|-----------|----------|---------|---------------|
| 原矿 | — | 1× | 自动采集点基础产出 |
| 粉碎 (Macerator) | Steam Macerator → Macerator | 2× | 建造粉碎机 → 采集点产出 ×2 |
| 清洗 (Ore Washer) | Ore Washer | 2.25× | — |
| 离心 (Centrifuge) | Centrifuge | 2.5× | — |
| 研磨 (Macerator 2) | Macerator (with Grinding Ball) | ~3× | 升级粉碎机 |
| 热离 (Thermal Centrifuge) | Thermal Centrifuge | ~3.5× | — |
| 多方块研磨 (Steam Grinder) | Steam Grinder | 125% 速度 + 8 并行 | 蒸汽多方块升级 |
| 多方块矿石处理 (GT++ Ore Processor) | GT++ Multis | 高并行 | EV+ 矿石处理阵列 |

> 设计意图：矿石增产是 GTNH 的核心进度线之一——从 Steam 时代的 2× 到 EV 时代的并行多方块阵列。GregIdle 不必模拟每一步，但需保留"建造/升级机器 → 资源产出倍率提升"的核心反馈。

### 7.5 电路系统抽象

GTNH 的电路 tier 不在 GregIdle 的第一版范围内做独立的电路生产链，但应保留"电路等级不同步于电压等级"的核心概念：

| 电路等级 | 对应电压时代 | 主要门槛 |
|---------|------------|---------|
| Primitive (Level 0) | Stone→Steam | 真空管、树脂 |
| Basic (Level 1) | LV | 铜线、钢、基础流体 |
| Good (Level 2) | MV | 基础化工、早期 SMD |
| Advanced (Level 3) | HV | 塑料、高级 SMD |
| Data (Level 4) | EV | 处理器、铂族贵金属 |
| Energy (Level 5) | IV | 量子晶元、PBI |
| Crystal (Level 6) | LuV/ZPM | 聚变产出的贵金属 |
| Quantum (Level 7) | UV | 生物计算 |
| Spacetime (Level 8+) | UHV+ | 时空材料 |

> GTNH Dev-Doc 明确指出"电路 tier 与电压 tier 不同步"——例如 HV 时代就已经制作 IV 电路。GregIdle 中，电路等级可作为任务奖励解锁配方/机器的门槛条件之一。

---

## 八、GTNH 设计哲学继承

> 以下原则提取自 GTNH Dev-Doc (vision of the modpack) 和 Developer's Code of Conduct，作为 GregIdle 在简化 GTNH 时应当保留的精神内核。

### 8.1 核心设计原则（引自 GTNH Dev-Doc）

| 原则 | 原文精神 | GregIdle 继承 |
|------|---------|--------------|
| **长线渐进式体验** | "long-lasting experience, tying mods together in progressive fashion" | 章节化长线推进，Stone→UXV |
| **Tier 门控** | "Tier materials are used to gate content" | 解锁条件中的材料检查 |
| **电路不同步** | "Circuit tiers don't correspond to voltage tiers" | 保留电路等级≠电压等级 |
| **先难后易** | "Materials hard to get in one tier become easier in later tiers" | 高时代自动化降低前期材料成本 |
| **避免小粉/迷你材料** | "Avoid small/tiny dusts if possible" | 配方输入输出直接取整 |
| **QoL 渐进解锁** | "QoL should not be gated unless rationale exists; simplify after player has done it dozens of times" | 任务奖励从手动→自动→批量 |
| **魔法不锁科技** | "One should be able to reach the end of the tech tree without doing any magic" | 魔法支线不进入 Tech Tree 主线 |
| **多个可选路径** | "Multiple alternative options — each with own advantages" | 不同产线路线可选 |
| **禁止重复堆叠** | "Power generation should not be repetitive spam" | 鼓励产线多样性，非同一机器堆叠 |
| **不鼓励过度 AFK** | "Avoid encouraging extensive AFK or large machine spam" | 离线收益结算、合理的机器数量规模 |
| **Tedium 定义** | "A required process that is not fun. Can be solved by tiering." | 早期手动→中期自动化→后期批量——自动化奖励就是 tiering 的 GregIdle 翻译 |
| **QoL 定义** | "Using pack code to automate tedium away. Simplify after player has done it dozens of times." | 任务奖励发放 QoL 能力，每一步都先让玩家感受一次 GTNH 味道 |

### 8.2 电力设计原则（引自 GTNH Dev-Doc Power Vision）

| 原则 | GTNH Dev-Doc 原文 | GregIdle 应用 |
|------|------------------|--------------|
| **多种电力选项** | "Use multiple different power options — same power source should not be good for more than 2-3 tiers" | 每个时代引入新发电类型：Steam→Oil/Benzene→Nuclear→Fusion |
| **备选路径** | "Multiple alternative options — each with own advantages" | 同一时代允许多种发电选择（如 LV 可选 Steam 缓冲或 Oil Distillery） |
| **简洁不堆叠** | "Should not be repetitive spam of same setup" | 发电设施数量有合理上限，鼓励升级而非堆叠 |
| **复杂度奖励** | "Simpler setups should always be less powerful than more complex ones" | 复杂产线→更高效率或净产出 |
| **副产品与协同** | "Byproducts and synergies — fuel may yield valuable byproducts" | 某些发电设施的燃料产线同时产出其他有用材料 |
| **可再生性** | "Renewable setups make less power than non-renewable ones" | 可再生发电效率略低于有限资源发电 |

### 8.3 GTNH 社区与贡献文化

GTNH 是一个**社区驱动的开源项目**，其贡献文化和社区结构值得 GregIdle 在面向社区分享时理解和尊重：

- **8+ 年持续开发**：GTNH 从 Minecraft 1.7.10 时代持续活跃开发至今，有数千条定制配方、自定义 Quest Book（46 QuestLine / 3749 节点）、独特世界生成和专门为整合包编写的模组
- **严格的贡献规范**：新内容需在 `#meta-dev` Discord 频道讨论并获得共识后才能提交 PR；平衡性 PR 需明确回答"目标是什么 / 副作用是什么 / 有没有数据支撑"
- **IV 以下不轻易加新内容**：Dev-Doc 明确规定"避免在 IV 及以下添加新内容，已有足够密度"
- **FOSS 原则**：所有新加入的模组必须是自由/开源软件（无例外）；大型模组需有维护者愿意与团队持续合作
- **社区角色体系**：从 Beta Tester → Contributor → Dev → Staff → Admin 的完整晋升路径，Dev 需遵守 Code of Conduct
- **尊重愿景文档**：所有贡献都需对照 vision doc 进行讨论和更新，vision doc 是 GTNH 的"宪法"

> GregIdle 是 GTNH 社区的 fan project，不隶属于 GTNH 官方团队。在分享和发布时，应尊重 GTNH 的社区文化和贡献者归属，保留 credits 声明和非商业性质。

---

## 九、存档机制

```
三层防护：

1. localStorage（主存档）
   - 每 30 秒通过 Zustand persist 自动写入
   - 键名: greg-idle-save-v1
   - 格式: JSON { version, timestamp, gameState, checksum }

2. IndexedDB（备份）
   - 每 5 分钟自动备份
   - 保留最近 10 份历史备份
   - 防 localStorage 被浏览器清理

3. 手动导出/导入
   - 导出为 .json 文件下载
   - 支持粘贴存档字符串导入
   - 玩家间可分享存档
```

### 存档迁移

键名 `greg-idle-save-v1` 里的 `v1` 是 schema 版本。朋友们会有持续存档，跨 Phase 改数据结构（新增字段、改材料 id、改电路等级语义）时旧档会崩，因此从一开始就约定迁移路径：

```typescript
// 每份存档头部记录写入时的 schema 版本
interface SaveEnvelope {
  schemaVersion: number;   // 当前 1
  timestamp: number;
  gameState: PlayerState;  // Decimal 字段经自定义 serializer 序列化为字符串
  checksum: string;
}

// 读档时按版本顺序逐级迁移到当前版本，再交给引擎
function migrate(save: SaveEnvelope): SaveEnvelope { /* v1→v2→…→current */ }
```

- 每次破坏性改 schema → `schemaVersion +1`，并补一个 `vN→vN+1` 迁移函数；迁移按版本顺序串联执行。
- 读到比当前代码更高的版本（玩家用了更新的部署）→ 拒绝加载并提示，避免静默吞档。
- `break_infinity.js` 的 Decimal 不是原生 JSON 类型，persist 需配自定义 `serialize/deserialize`——这点与大数选型（技术选型表）绑定，Phase 0 一起定。

---

## 十、游戏数据规模预估（Phase 2 完成后）

| 类别 | 数量 | 备注 |
|------|------|------|
| 资源/材料 | ~80-120 种 | 含材料形态变体（锭/板/杆/线/粉）+ 流体/气体 |
| 机器类型 | ~20-30 种 | 含手动站、采集点、加工机、能源生产/转换 |
| 配方 | ~150-200 条 | 含副产物——每条配方平均 1-2 个副产物 |
| 时代/电压章节 | Stone + Steam + 13 个电压章节 | 共计 15 个 Era（含 Stone / Steam / LV→UXV） |
| 任务节点 | ~80-150 个 | 平均每时代 5-10 个关键节点 |
| Roadmap 里程碑 | ~15 个 | 每时代 1 个核心里程碑 |
| 电路等级 | ~9 级 | Primitive → Basic → Good → Advanced → Data → Energy → Crystal → Quantum → Spacetime |
| 火箭/航天 | 远期可选 | 不进入 Phase 1/2，作为 Prestige 或 Phase 3+ |
| Prestige 升级 | 远期可选 | 不进入 Phase 1/2 |

> 以上为高度抽象后的放置游戏规模。真实 GTNH 2.9.0-beta-1 含数百种资源/机器、数千条配方、46 条 QuestLine（3749 个任务节点）、13 个电压 tier（ULV→MAX）、9+ 个电路等级。GregIdle 的目标是在保留产线深度、GTNH 核心节奏和关键痛点（副产物、电压机制、电路不同步、矿石增产链）的前提下，将规模控制在放置游戏可管理的范围。

---

## 十一、开发路线图

### Phase 0：骨架搭建（预计 1 周）

- [ ] Vite + React + TypeScript + Tailwind 项目初始化
- [ ] 目录结构搭建
- [ ] `src/data/` 核心类型与基础数据定义
- [ ] `src/engine/` tick / recipe / era / quest 骨架
- [ ] Zustand store 初始化 + persist 中间件（含 Decimal 自定义 serialize/deserialize）
- [ ] 大数底座：接入 `break_infinity.js`，引擎算式与存档统一走 Decimal（先定，后面全靠它）
- [ ] 存档系统（storage.ts）+ `SaveEnvelope` schema 版本头 + `migrate()` 骨架
- [ ] 大数格式化工具（format.ts，K/M/B/T/Qa…）
- [ ] Vitest 接入 + 引擎核心单测骨架（tick 推进 / 超频倍率 / 离线结算数值自洽）

### Phase 1：最小可玩版（预计 3-4 周）

- [ ] 三栏布局 + TopBar / BottomBar 框架
- [ ] ResourcePanel + ResourceRow 组件
- [ ] WorkshopPanel + MachineCard + RecipeSelector 组件
- [ ] QuestBookPanel + QuestEraTabs + EraRoadmap 组件
- [ ] `useGameLoop` tick 主循环
- [ ] `useOfflineCalc` 离线结算
- [ ] Stone Age → Steam Age → LV 入门
- [ ] 前 5 分钟手动采集，之后解锁自动采集点和工作台队列
- [ ] 第一版终点：造出第一台 LV 电力机器
- [ ] 20-30 种资源 + 8-12 种机器/设施 + 30-45 条配方
- [ ] 工业仪表盘 CSS 主题
- [ ] 存档/读档 + 导出/导入

### Phase 2：深度填充（预计 4-6 周）

- [ ] LV → MV → HV 电压扩展
- [ ] 50+ 资源/材料 + 20 种机器
- [ ] 基础化学线（橡胶/酸/基础流体处理）
- [ ] 完整材料加工链（ore→crushed→purified→dust→ingot→plate→rod→wire）
- [ ] 副产物管理系统
- [ ] 时代任务图扩展到 HV
- [ ] 数据统计面板（总产量、效率榜、历史）
- [ ] ProgressBar / PowerGauge 动画打磨

### Phase 3：中后期扩展（持续迭代）

- [ ] EV → IV → LuV → ZPM → UV → UHV → UEV → UIV → UMV → UXV 分阶段扩展
- [ ] 高阶自动化、AE/批量队列、终局材料与奇点目标
- [ ] framer-motion 动画：数字跳动、进度发光、机器指示灯
- [ ] 移动端响应式适配
- [ ] 数值平衡调优
- [ ] 音效（可选）
- [ ] 熟人玩家导向的任务书文案与提示密度调优
- [ ] 成就系统
- [ ] i18n 国际化（中/英）

---

## 十二、技术决策记录

| 日期 | 决策 | 理由 |
|------|------|------|
| 2026-06-19 | Web 网页游戏 | 迭代快、分发零门槛 |
| 2026-06-19 | 硬核深度（C 级） | 面向上千小时的硬核放置玩家 |
| 2026-06-19 | 工业仪表盘风（B 级） | GTNH 原生 UI 审美 + 开发性价比最高 |
| 2026-06-19 | React + TypeScript | 组件化天然适合仪表盘；海量状态管理成熟 |
| 2026-06-19 | 纯前端单机 | 参考 Melvor Idle 验证的成功路径 |
| 2026-06-19 | 纯仪表盘面板式架构 | 开发效率最高，预留节点图接口后期扩展 |
| 2026-06-19 | Zustand 状态管理 | 比 Redux 轻量，selector 精准渲染，persist 内置 |
| 2026-06-19 | 非商业 fan project | 面向熟悉 GTNH 的朋友/社区玩家，不收费、不做广告、不冒充官方 |
| 2026-06-19 | Quest Book + Roadmap | 任务书负责当前目标，Roadmap 负责 Stone→UXV 的长期位置 |
| 2026-06-19 | 项目命名 GregIdle | GTNH 社区内部分享，Greg 梗亲切简短好记，仓库 / title / 存档键名统一 |
| 2026-06-19 | 数据锚定 GTNH 2.9.0-beta-1 | 电压、蒸汽换算、超频倍率、材料归属均对照 GT5-Unofficial 源码 + Dev-Doc + Wiki 交叉核对 |
| 2026-06-19 | GregIdle 是放置翻译非复刻 | 保留 GTNH 核心体验（副产物、电压、产线规划），将数千配方抽象为放置可玩规模 |

---

## 十三、风险与缓解

| 风险 | 影响 | 缓解措施 |
|------|------|---------|
| 数值平衡失控 | 某些产线过强/过弱，破坏节奏 | Phase 1 数据少时容易手动调；Phase 2 以后建 Excel 模拟器辅助 |
| localStorage 被清 | 玩家进度丢失 | IndexedDB 双备份 + 提醒定期导出 |
| 大数溢出 | 资源数量超过 `Number.MAX_SAFE_INTEGER` | 使用 BigInt 或 `break_infinity.js` 大数库 |
| 性能瓶颈 | 200 机器 × 16ms tick 卡帧 | Zustand selector 精准渲染 + tick 逻辑纯函数高效 + Web Worker 可选 |
| 离线结算被时钟欺骗 | 手动改系统时钟获取不当离线收益 | 存档内记录 `lastKnownWallClock`——当时间回退或离线跨度异常时降级到保守估算，写入一次警告标记 |
| GTNH 素材/署名 | fan project 分享时仍需尊重原作与素材许可 | 保留非商业声明、credits、素材来源清单；若公开发布，先替换不明确授权的素材 |
| 远景过大 | Stone→UXV 内容量巨大，容易长期停留在设计稿 | Phase 1 只验证 Stone→Steam→LV 入门；后续每次只扩 1-2 个时代章节 |
| GTNH 真实性漂移 | 迭代中数值和内容偏离 GTNH 原作精神 | 每新增时代对照 GTNH-Insight progression 复核材料、机器和痛点；电压/配方数据锚定 GT5-Unofficial 源码 |
| 社区关系 | fan project 可能被误认为官方或引起社区争议 | 明确标注非官方 + 非商业 + credits；尊重 GTNH Dev-Doc 和 Code of Conduct；遵守 GTNH 社区规范和素材许可 |

---

## 十四、参考数据源与致谢

### 数据锚定来源

| 来源 | 用途 | 关联 |
|------|------|------|
| **GTNH 2.9.0-beta-1** | 数据锚定版本——电压、配方、材料归属 | 游戏内 Quest Book 46 QuestLine / 3749 节点 |
| **GT5-Unofficial 5.09.52.594** | 核心数据（`GTValues.V`、`GTValues.VP`、OverclockCalculator） | 电压档位基准 |
| **GTNH Dev-Doc** | 16 阶段设计意图、主要挑战、终期条件 | GTNH-Insight 知识库 |
| **GTNH Developer's Code of Conduct** | 开发者行为规范、PR 审查标准、贡献文化 | GTNH 社区治理 |
| **GTNH Wiki** | 蒸汽换算、配方数据、材料链 | `gtnh.miraheze.org` |
| **GTNH Discord** | 社区反馈、更新公告模式、角色体系 | #announcements / #meta-dev |

### 致谢

GregIdle 是 GTNH 社区的 fan project，受益于 GTNH 团队 8+ 年的设计积累和开源贡献。以下是 GTNH 的核心贡献者群体：

- **DreamMasterXXL** — GTNH 项目领导者
- **GTNH Dev Team** — 全部开发者、Quest Editor、Texture Creator、Wiki Editor、Beta Tester
- **GTNH Contributor 社区** — 所有提交过 PR 和反馈的社区成员
- **GTNH-Insight** — 间接提供设计验证数据源

> GregIdle 不隶属于 GTNH 官方团队，不替代原作体验，不商业盈利。如果你觉得好玩，请去玩真正的 GTNH。
