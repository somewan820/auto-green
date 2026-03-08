# auto-green 知识库

> 本知识库记录仓库当前通过 GitHub Actions 定时创建空提交、维持默认分支活跃度的真实实现与约束。

## 快速导航

| 需要了解 | 读取文件 |
|---------|---------|
| 项目概况、技术栈、开发约定 | [context.md](context.md) |
| 模块索引 | [modules/_index.md](modules/_index.md) |
| 自动活跃工作流的职责与约束 | [modules/github-actions.md](modules/github-actions.md) |
| 项目变更历史 | [CHANGELOG.md](CHANGELOG.md) |
| 历史方案索引 | [archive/_index.md](archive/_index.md) |
| 当前待执行的方案 | [plan/](plan/) |
| 历史会话记录 | [sessions/](sessions/) |

## 模块关键词索引

> AI 可先读此表，再按需进入对应模块文档。

| 模块 | 关键词 | 摘要 |
|------|--------|------|
| github-actions | GitHub Actions, schedule, workflow_dispatch, empty commit, default branch | 通过定时或手动触发工作流，在仓库默认分支创建并推送空提交。 |

## 知识库状态

```yaml
kb_version: v2.2.3
最后更新: 2026-03-08 23:08
模块数量: 1
待执行方案: 0
```

## 读取指引

```yaml
启动任务:
  1. 读取本文件获取导航
  2. 读取 context.md 获取项目上下文
  3. 检查 plan/ 是否存在进行中的方案包

任务相关:
  - 涉及自动提交行为: 读取 modules/github-actions.md
  - 需要历史决策: 搜索 CHANGELOG.md → 读取对应 archive/{YYYY-MM}/{方案包}/proposal.md
  - 需要查看已完成方案: 读取 archive/2026-03/202603082244_auto-commit-fix/
```
