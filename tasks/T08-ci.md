# T08：CI workflow

> TODO 回链：[`../TODO.md`](../TODO.md) T08
> 状态：⬜ 未开始
> 认领窗口：—
> 类型：`配置`
> 文件锁：`.github/workflows/ci.yml`

---

## 目标

push 自动跑 lint + typecheck + test + build，首次推送后 Actions 全绿。

## 输入来源

- 依赖 T02（build）、T05（test）、T07（lint）
- [TODO.md](../TODO.md)

## 执行步骤

- [ ] 认领工单。
- [ ] `ci.yml`：on push/PR to main，Node setup，`npm ci`。
- [ ] 步骤：lint → `tsc --noEmit` → `vitest run` → `vite build`。
- [ ] 推送验证 Actions 绿。
- [ ] DoD 自检。

## DoD

- [ ] 首次 push 后 GitHub Actions 全绿。
- [ ] 四步（lint/typecheck/test/build）均跑。
- [ ] 未写入本地绝对路径。

## 进度记录

- 初始：待认领。

## 产出链接

- 暂无。
