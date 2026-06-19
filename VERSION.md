# 版本锚定 (Version Anchor)

> **本文件是 GregIdle 数值设计的「单一事实来源 (Single Source of Truth)」。**
> 每一处来自 GTNH 的数值（电压、超频倍率、蒸汽换算、材料归属等）都必须可回溯到本文件锁定的版本。
> 源码会随版本变化，Wiki / Dev-Doc 也会更新——**没有版本锚定的数值结论无效**。

> GregIdle 与 GTNH-Insight 共享同一份 `_local/` 参考数据，版本锚定**直接复用** GTNH-Insight 已锁定的值，保证两个项目的数值口径一致。

---

## 1. 顶层锚点：整合包 Release

| 项目 | 值 |
|------|-----|
| 整合包 | GregTech: New Horizons (GTNH) |
| **基准 Release** | `2.9.0-beta-1` |
| 分支 | Java 17-25 |
| 完整包名 | `GT_New_Horizons_2.9.0-beta-1_Java_17-25` |
| Release 日期 | 2026-06-08 |
| 下载 / 版本历史 | https://www.gtnewhorizons.com/version-history |

> ⚠️ **这是 beta 版，锚定方式比 stable 更严格。**
> beta 是滚动开发版：同一个 `2.9.0-beta-1` 名字下，整合包内的 mod 版本可能被多次刷新。
> 因此整合包名字不足以唯一确定代码——真正的锚点是下面第 3 节里 GT5-Unofficial 的**确切版本号 + commit**，外加查证日期。

---

## 2. 为什么不能用「GTNH 的 commit hash」

GTNH 是由约 400 个 mod 组装的**整合包**，没有单一源码仓库、没有单一 commit。正确的锚定是两层：

```
整合包 Release (2.9.0-beta-1)   ← 顶层锚点，人类可读（但 beta 滚动更新，不唯一）
        └── 某个具体 mod 的确切版本 (GT5-Unofficial 5.09.xx.xxx)  ← beta 下以此为准
                └── 该 mod 仓库对应的 git tag / commit  ← 真正 checkout 读源码的东西
```

GregIdle 的数值几乎全部来自 GT5-Unofficial 这一个 mod：

| Mod | 仓库 | 提供的内容 |
|-----|------|-----------|
| **GT5-Unofficial** | https://github.com/GTNewHorizons/GT5-Unofficial | GT 机器 / MTE、配方引擎、超频、电压、StructureLib 调用 |
| StructureLib | https://github.com/GTNewHorizons/StructureLib | 多方块结构定义与校验框架 |
| GTNewHorizonsCoreMod | https://github.com/GTNewHorizons/NewHorizonsCoreMod | GTNH 魔改 / 自定义配方脚本 |

---

## 3. 锁定记录（已冻结）

> GregIdle 所有 GTNH 数值引用均以此为准。

| 项目 | 值 | 状态 |
|------|-----|------|
| 整合包版本 | `2.9.0-beta-1` (Java 17-25) | ✅ 已定 |
| GT5-Unofficial 确切版本 | `5.09.52.594`（jar：`gregtech-5.09.52.594.jar`）| ✅ 已锁 |
| GT5-Unofficial git tag / commit | tag `5.09.52.594` / commit `5c12757bbc033d0e030740faeb6e5604b94d5e40` | ✅ 已锁 |
| StructureLib | `1.4.38`（tag `1.4.38`）| ✅ 已锁 |
| gtnhlib | `0.11.9`（tag `0.11.9`）| ✅ 已锁 |
| GTNewHorizonsCoreMod | `2.8.279`（jar：`GTNewHorizonsCoreMod-2.8.279.jar`；源码：`_local/nhcoremod-src/`）| ✅ 已锁 |
| GTNH-Dev-Doc commit | `f38451ff4436bba15c9d4610a416a471c2c1110d` | ✅ 已锁 |
| 查证日期 | 2026-06-10 | ✅ |

> 源码已克隆到本地参考区（不入库）：`_local/gt5u-src/`、`_local/structurelib-src/`、`_local/gtnhlib-src/`、`_local/nhcoremod-src/`，均对应上述锁定版本。完整 mod 清单见 `_local/mods-manifest.txt`（241 个）。

> ⚠️ **这些源码树是「脱离 git 的快照」**（无各自的 `.git`），本机无法用 `git` 命令直接证明它们就是上表的 tag/commit。因此**数值溯源以「内容 + `文件:行号`」为准，不依赖 git 校验**（见 [docs/data-sourcing.md](docs/data-sourcing.md)）。若需可验证性，可按上表 tag 重新 `git clone` 带 `.git` 的 checkout 比对。引用源码时务必标 `文件:行号`，行号对应上表锁定版本。

---

## 4. GregIdle 设计文档锚点

| 项目 | 值 |
|------|-----|
| 设计文档 | `docs/gtnh-idle-game-design.md` |
| 当前版本 | v1.3（2026-06-19） |

> 设计文档中的「真实性声明」(§1) 即以本文件锁定的版本为准。

---

## 5. 升级版本时的复核流程

当锚定的 GTNH / GT5U 版本变化时：

1. 更新本文件第 1、3 节的版本号与 commit，并刷新查证日期。
2. 重新提取 jar / config，按新版本号重新拉取 `_local/` 下的源码树。
3. 在 [STATUS.md](STATUS.md) 标注「数值需复核」，并对照 [docs/data-sourcing.md](docs/data-sourcing.md) 的已锚定数值清单逐项核对。
4. 复核重点：电压档位（`GTValues.V` / `VP`）、超频倍率、蒸汽换算、材料归属——这些是设计文档数值的根。

---

## 6. 数值溯源声明模板

在数据文件（`src/data/*.ts`）或设计文档中引用 GTNH 数值时，按此格式标注出处：

```
// 来源：GTNH 2.9.0-beta-1 / GT5-Unofficial 5.09.52.594
// gt5u-src/.../GTValues.java:<行号>
```

详细溯源规范见 [docs/data-sourcing.md](docs/data-sourcing.md)。
