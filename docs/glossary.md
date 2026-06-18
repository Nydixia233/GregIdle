# 术语表 (Glossary)

> GTNH 术语 → 全称 + 在 GregIdle 里的作用。给协作者快速对齐，不必通读设计文档。
> 数值口径以 [VERSION.md](../VERSION.md) 锁定版本为准。

---

## 电压等级（Voltage Tier）

GregIdle 电压从 LV 起，Roadmap 终点 UXV。每级为上一级 4×。数值见 [data-sourcing.md](data-sourcing.md)。

| 简称 | 全称 | EU/t |
|------|------|------|
| ULV | Ultra Low Voltage | 8（内部边界，不入主线） |
| LV | Low Voltage | 32 |
| MV | Medium Voltage | 128 |
| HV | High Voltage | 512 |
| EV | Extreme Voltage | 2,048 |
| IV | Insane Voltage | 8,192 |
| LuV | Ludicrous Voltage | 32,768 |
| ZPM | Zero Point Module | 131,072 |
| UV | Ultimate Voltage | 524,288 |
| UHV→UXV | Ultra High → Ultimate Extreme | 2,097,152 起，每级 4× 到 536,870,912 |

---

## 机器 / 多方块

| 简称 | 全称 | 作用 |
|------|------|------|
| EBF | Electric Blast Furnace（电力高炉） | 高温冶炼，LV→MV 软卡点；GregIdle 抽象为「建造机器」 |
| BBF | Bricked Blast Furnace | 蒸汽时代炼钢前置 |
| LCR | Large Chemical Reactor（大型化学反应釜） | HV 化工核心，无电池化工 |
| Cleanroom | 无尘室 | HV 门控：先建无尘室才能做高阶电路 |
| Vacuum Freezer | 真空冷冻机 | 热锭快速冷却（如钛、钨钢） |
| Assembly Line | 装配线 | EV→IV 最重磅建造项目 |
| CAL | Circuit Assembly Line | 大幅加快电路生产（LuV） |
| Fusion Reactor | 聚变反应堆 | LuV 起的高能发电 |
| Macerator | 粉碎机 | 矿石增产链第一步（×2） |

---

## 材料 / 化学

| 简称 / 词 | 含义 |
|-----------|------|
| PGM | Platinum Group Metals（铂族金属：Pt/Pd/Rh/Os/Ir/Ru），IV 最复杂化学链 |
| Naquadah | 后期核心材料线（LuV 起），含 Enriched Naquadah / Naquadria |
| Tungstensteel | 钨钢，EV 终点材料（一叠为 EV→IV 门槛） |
| PTFE / PBI / PPS | 高阶聚合物（特氟龙 / 聚苯并咪唑 / 聚苯硫醚），后期化工必需 |
| Stainless Steel | 不锈钢，HV 主材料 |

---

## 机制术语

| 词 | 含义 |
|----|------|
| Overclock (OC) | 超频：提电压换速度。标准每次 EU ×4、耗时 ÷2。GregIdle 用 `getVoltageEffect()` 简化映射 |
| Perfect OC | 完美超频：耗时 ÷4（如 EBF 达温度阈值），GregIdle 暂不细分 |
| Byproduct | 副产物：一条配方除主产物外的额外产出，GTNH 核心特色（设计文档反复强调保留） |
| Ore doubling | 矿石增产：粉碎→清洗→离心→研磨的多阶段增产链，GregIdle 抽象为机器升级倍率 |
| Tier gating | 时代门控：用某 tier 材料 / 电路限制更高 tier 内容解锁 |
| Circuit tier ≠ Voltage tier | 电路等级不同步于电压等级（如 HV 时代做 IV 电路），GTNH 核心体验 |

---

## 项目内部约定

| 词 | 含义 |
|----|------|
| 放置翻译 | GregIdle 的定位：忠实 GTNH 材料链与节奏，但抽象为放置可玩规模，非 1:1 复刻 |
| 有效游玩时间 | 节奏曲线的时间锚：在线 + 离线结算累计的活跃推进时长，非日历时间（设计文档 §7.1.1） |
| 数值锚定 | 所有 GTNH 数值回溯到 VERSION.md 锁定版本，标 `文件:行号` |
| `_local/` | 版权 / 大体积参考资料工作台，除自身 README 外永不入库 |
