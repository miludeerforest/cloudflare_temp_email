# Github Actions 部署如何配置自动更新

::: warning 注意
有问题请通过 `Github Issues` 反馈，感谢。
自动更新不会执行 D1 数据库的 sql 文件，当数据库 schema 变动时，需要手动执行。
:::

1. 在 GitHub 创建 Fine-grained personal access token，仅授权当前 Fork 仓库，并将仓库权限 **Contents** 和 **Workflows** 设为 **Read and write**。设置合适的有效期，过期前及时更换。
2. 打开当前仓库的 `Settings → Secrets and variables → Actions`，新增 Repository secret，名称为 `SYNC_TOKEN`，值为上述令牌。不要将令牌提交到代码或粘贴到聊天、Issue、日志中。
3. 打开仓库的 `Actions` 页面，找到 `Upstream Sync`，启用后点击 `Run workflow`。同步成功后，现有流程会按同步后的提交 SHA 部署前后端，并记录成功部署的版本。
4. 如果上游包含 workflow 文件变更，默认 `GITHUB_TOKEN` 无法替代此同步令牌；Cloudflare API Token 也不能提供 GitHub 同步权限。失败时先检查同步日志、`SYNC_TOKEN` 的有效期及仓库授权，不要强制覆盖 Fork 的改动。
5. 修改 `Upstream Sync` 的 `schedule` 配置可自定义更新间隔，参考 [cron 表达式](https://crontab.guru/)
6. `Deploy Production` 的标签推送入口仅匹配 `v*` 应用版本标签，`mail-parser-wasm-v*` 包发布标签不会触发应用部署；同步后的调用入口和手动部署入口保持不变。
