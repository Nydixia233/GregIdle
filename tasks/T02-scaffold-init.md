# T02：Vite + React + TS + Tailwind 初始化

> TODO 回链：[`../TODO.md`](../TODO.md) T02
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`脚手架`
> 文件锁：`package.json` / `vite.config.ts` / `tsconfig.json` / `tailwind.config.ts` / `postcss.config.js` / `index.html` / `src/main.tsx` / `src/App.tsx`

---

## 目标

初始化 Vite + React 18 + TypeScript + Tailwind 项目，`pnpm dev` 能起空白页，目录结构按设计文档 §3 落地。

## 输入来源

- [docs/gtnh-idle-game-design.md](../docs/gtnh-idle-game-design.md) §2 技术选型 / §3 项目结构 / §4.6 CSS 变量
- [AGENTS.md](../AGENTS.md) §4 代码规范
- [TODO.md](../TODO.md)

## 执行步骤

- [ ] 认领工单，更新 TODO 与本文件状态。
- [ ] `pnpm create vite@latest` 选 react-ts，或手配 package.json。
- [ ] 配 Tailwind + PostCSS，注入 §4.6 工业风 CSS 变量。
- [ ] TS 严格模式（`strict: true`）。
- [ ] 建 §3 目录骨架：`src/{engine,data,stores,components,hooks,utils}`。
- [ ] DoD 自检。

## DoD

- [ ] `pnpm dev` 启动无报错，渲染空白页。
- [ ] `tsc --noEmit` 通过，strict 开。
- [ ] 目录结构与设计文档 §3 一致。
- [ ] CSS 变量可用（§4.6）。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
