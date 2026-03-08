# 变更日志

## [Unreleased]

## [0.1.1] - 2026-03-08

### 修复
- **[github-actions]**: 修复默认分支自动空提交流程，显式授予 `contents: write`、检出并推送仓库默认分支，并统一使用 UTC 时间戳提交消息；同步更新 `README.md` 说明当前行为、失效原因和使用前检查。
  - 方案: [202603082244_auto-commit-fix](archive/2026-03/202603082244_auto-commit-fix/)
  - 决策: auto-commit-fix#D001（采用 GitHub Actions 定时空提交维持默认分支活跃度）
