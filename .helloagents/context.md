# 项目上下文

## 1. 基本信息

```yaml
名称: auto-green
描述: 使用 GitHub Actions 定时创建空提交，保持仓库默认分支的提交活跃度。
类型: 自动化仓库 / GitHub Actions 工作流配置
状态: 维护中
```

## 2. 技术上下文

```yaml
语言: YAML, Shell, Git
框架: GitHub Actions
包管理器: 无
构建工具: 无
```

### 主要依赖
| 依赖 | 版本 | 用途 |
|------|------|------|
| actions/checkout | v4 | 检出仓库默认分支，供后续空提交与推送使用 |
| GitHub Actions runner | ubuntu-latest | 执行 workflow 中的 Git 和 Shell 步骤 |

## 3. 项目概述

### 核心功能
- 按 `17 */3 * * *`（UTC）定时触发自动活跃工作流。
- 支持 `workflow_dispatch` 手动触发同一流程。
- 检出仓库默认分支，创建 `chore: auto green <UTC timestamp>` 空提交并推送回默认分支。
- 在 `README.md` 中说明当前行为、历史失效原因与使用前检查项。

### 项目边界
```yaml
范围内:
  - 维护 GitHub Actions 自动空提交流程
  - 维护与该流程一致的 README 使用说明
  - 依赖默认分支进行活跃度维护
范围外:
  - 业务应用代码开发
  - 构建、测试、发布产物
  - 第三方接口集成或动态提交内容生成
```

## 4. 开发约定

### 代码规范
```yaml
命名风格: 工作流名称与仓库名保持一致，自动提交消息固定前缀为 chore: auto green
文件命名: GitHub Actions 工作流位于 .github/workflows/，当前主文件为 ci.yml
目录组织: 根目录以 README.md 说明行为，自动化逻辑集中在 .github/workflows/ci.yml
```

### 错误处理
```yaml
错误码格式: 无应用级错误码
日志级别: 以 GitHub Actions step 日志为准，用于定位 checkout、commit、push 失败原因
```

### 测试要求
```yaml
测试框架: 无
覆盖率要求: 无
测试文件位置: 无，主要通过 workflow_dispatch 或定时运行结果验证
```

### Git规范
```yaml
分支策略: 自动化流程始终针对仓库默认分支执行
提交格式: chore: auto green <UTC timestamp>
```

## 5. 当前约束（源自历史决策）

> 这些约束由当前修复方案固定，文档与代码必须保持一致。

| 约束 | 原因 | 决策来源 |
|------|------|---------|
| 工作流必须声明 `permissions.contents: write` | 需要将空提交直接推送回仓库默认分支 | [auto-commit-fix#D001](archive/2026-03/202603082244_auto-commit-fix/proposal.md#D001) |
| checkout 与 push 的目标都使用 `${{ github.event.repository.default_branch }}` | 避免写死分支名，保证流程跟随仓库默认分支 | [auto-commit-fix#D001](archive/2026-03/202603082244_auto-commit-fix/proposal.md#D001) |
| 自动提交消息必须包含 UTC 时间戳 | 便于审计每次空提交的实际触发时间 | [auto-commit-fix#D001](archive/2026-03/202603082244_auto-commit-fix/proposal.md#D001) |
| README 必须同步记录失效原因与使用前检查 | 该仓库的可维护性主要依赖工作流说明而非应用代码 | [auto-commit-fix#D001](archive/2026-03/202603082244_auto-commit-fix/proposal.md#D001) |
