# github-actions

## 职责

`github-actions` 模块负责定义仓库当前唯一的自动化行为：通过 GitHub Actions 在仓库默认分支上定时或手动创建空提交，以维持默认分支的提交活跃度。

该模块的真实实现位于 `.github/workflows/ci.yml`，并与 `README.md` 中的行为说明保持一致。

## 接口定义（可选）

### 触发入口
| 入口 | 位置 | 说明 |
|------|------|------|
| `workflow_dispatch` | `.github/workflows/ci.yml` | 允许在 GitHub Actions 页面手动触发一次流程 |
| `schedule` | `.github/workflows/ci.yml` | 按 `17 */3 * * *`（UTC）每 3 小时触发一次 |

### 关键配置
| 配置项 | 当前值 | 说明 |
|--------|--------|------|
| `name` | `auto-green` | 工作流名称 |
| `permissions.contents` | `write` | 允许 workflow 使用令牌推送空提交 |
| `concurrency.group` | `auto-green-${{ github.repository }}` | 以仓库维度串行化运行 |
| `concurrency.cancel-in-progress` | `false` | 不取消已在执行中的任务 |
| `actions/checkout` | `v4` | 检出仓库默认分支，`fetch-depth: 0` |
| 目标分支 | `${{ github.event.repository.default_branch }}` | checkout 与 push 共用的目标分支 |
| 提交消息 | `chore: auto green <UTC timestamp>` | 通过 UTC 时间戳标记每次空提交 |

## 行为规范

### 定时触发
**条件**: `schedule` 命中 `17 */3 * * *`
**行为**: 检出默认分支，配置 Git 身份，创建空提交并推送到默认分支
**结果**: 默认分支新增一条 `chore: auto green <UTC timestamp>` 空提交

### 手动触发
**条件**: 用户在 GitHub Actions 页面执行 `workflow_dispatch`
**行为**: 运行与定时任务相同的 checkout、commit、push 流程
**结果**: 可立即验证当前仓库是否仍能成功完成自动空提交

### 关键约束
**条件**: 工作流需要成功将提交写回默认分支
**行为**: 必须保留 `permissions.contents: write`、默认分支检出与默认分支推送
**结果**: 避免因权限不足或目标分支错误导致流程失效

### 外部依赖最小化
**条件**: 需要保持流程稳定运行
**行为**: 不依赖外部语录 API、随机执行逻辑或额外生成步骤
**结果**: 失败面主要收敛为 GitHub Actions 权限、默认分支状态与仓库配置问题

## 注意事项

- GitHub Actions 必须处于启用状态，否则定时任务不会运行。
- 提交邮箱 `2176978853@qq.com` 需要绑定并验证到对应 GitHub 账号，否则提交可能不会计入贡献图。
- GitHub 对长期无活动的定时工作流可能自动暂停，暂停后需要在 Actions 页面重新启用。
- `README.md` 中的当前配置、失效原因和使用前检查必须与 `.github/workflows/ci.yml` 保持一致。

## 依赖关系

```yaml
依赖:
  - GitHub Actions
  - actions/checkout@v4
  - Git CLI
  - 仓库默认分支
被依赖:
  - README.md（行为说明）
  - 默认分支活跃度维护流程
```
