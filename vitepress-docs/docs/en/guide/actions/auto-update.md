# How to Configure Auto-Update for GitHub Actions Deployment

::: warning Notice
If you encounter any issues, please report them via `GitHub Issues`. Thank you.
Auto-update will not execute SQL files for the D1 database. When the database schema changes, you need to execute them manually.
:::

1. Create a GitHub fine-grained personal access token restricted to this fork, with **Contents** and **Workflows** repository permissions set to **Read and write**. Choose an appropriate expiration date and rotate the token before it expires.
2. Open this repository's `Settings → Secrets and variables → Actions` and add a repository secret named `SYNC_TOKEN` containing that token. Never commit the token or paste it into chats, issues, or logs.
3. Open the repository's `Actions` page, enable `Upstream Sync`, and select `Run workflow`. After synchronization, the existing workflow deploys the frontend and backend at the synchronized commit SHA and records the successfully deployed revision.
4. When upstream changes include workflow files, the default `GITHUB_TOKEN` cannot replace this sync token; a Cloudflare API token cannot grant GitHub sync permissions either. On failure, inspect the sync logs, token expiration, and repository access rather than force-overwriting fork changes.
5. Customize the update interval through the `schedule` configuration in `Upstream Sync`; see [cron expressions](https://crontab.guru/).
