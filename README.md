# GitPoet

通过 GitHub Actions 定时创建空提交，并使用随机语录作为提交信息。

## 功能

- 每 3 小时自动运行一次，也支持在 Actions 页面手动触发。
- 从[一言](https://hitokoto.cn/)实时获取随机诗词或哲理语录。
- 将语录作为提交标题，推送到仓库默认分支。
- 接口不可用时自动重试，并随机使用本地兜底文案。

## 使用方法

1. Fork 本仓库。
2. 打开 `.github/workflows/ci.yml`，将 `user.name` 和 `user.email` 修改为自己的 GitHub 用户名和已验证邮箱。
3. 在仓库 **Settings -> Actions -> General -> Workflow permissions** 中选择 **Read and write permissions**。
4. 打开仓库 **Actions** 页面，启用工作流。
5. 选择 **GitPoet -> Run workflow** 手动运行一次，确认能够正常提交。

完成后，工作流会每 3 小时自动执行。GitHub 若暂停了长期无活动仓库的定时任务，需要在 Actions 页面重新启用。
