# Star Gratitude ⭐

别忘记给你真正用着好用的 skill 和项目点个 star。

一个极简的 agent skill：提醒你给「用着好用」的第三方 skill 或项目点 GitHub star——因为「回头再 star」几乎永远不会发生。它会自动定位 skill 的来源仓库、检查你是否已经 star 过、征求你的确认后再执行，并把每次 star 记入本地 gratitude 账本，同一来源绝不重复打扰。

[English](README.md) · **简体中文**

## 工作原理

skill 在两种时机触发：

1. **第一次使用第三方 skill**——当某次会话第一次调用来自插件、`~/.agents/skills`、`~/.dsh/skills` 或项目 skills 目录的 skill 时，会轻量提醒一句它的来源，并问你是否想 star；
2. **明确称赞**——当你说某个 skill 或项目好用、值得收藏、值得 star 时，会解析出它的来源仓库并引导你完成 star。

两种情况都走同一套流程：

1. **定位仓库**：从 skill 目录（`SKILL.md` / `README.md` / `package.json` 的 `repository` 字段）、`git remote -v` 或联网搜索中找到来源仓库（`owner/repo`）。**绝不猜测仓库。**
2. **去重**：对照本地账本——已经 star 过或已经提醒过，就到此为止。
3. **先问后点**：star 是**你账号上的公开动作**，所以永远先征求你的明确同意。
4. **执行**：`gh api -X PUT user/starred/OWNER/REPO`。
5. **记账**：追加到 `~/.dsh/star-gratitude/log.md`（仅存本地，绝不推送）。

## 安装

需要 [GitHub CLI](https://cli.github.com/)（`gh`）且已登录并授权 `repo` scope（`gh auth login`）。

### DSH / 使用 `~/.agents/skills` 或 `~/.dsh/skills` 的 agents

```bash
git clone https://github.com/himhhh/star-gratitude.git ~/.agents/skills/star-gratitude
# 或
git clone https://github.com/himhhh/star-gratitude.git ~/.dsh/skills/star-gratitude
```

skill watcher 会自动发现，无需重启。

### 通用方式

把 `SKILL.md` 复制到你的 agent 能发现的任意 skill 根目录（如 `.agents/skills/`、`.dsh/skills/`、`skills/`）。

## 使用方法

- 说一句「这个 skill 真好用，想给它点个 star」「star 一下 xx 项目」，或者只是第一次调用某个第三方 skill——剩下的交给 agent，它永远会在 star 前征求你的确认。
- 账本位于 `~/.dsh/star-gratitude/log.md`，首次使用时自动创建。条目格式为 `日期 | owner/repo | 名称 | 为什么好用 | 状态`，其中状态为 `✅ starred`（已完成）或 `⏳ reminded`（提醒过未确认——之后不再重复问）。

## 安全规则

- star 是**公开动作**、发生在**你自己的账号**上——agent 会用 `gh auth status` 确认账号，并且永远先征求同意再执行。
- 已 star / 已提醒过 → 跳过，绝不重复打扰。
- **不猜仓库**：来源解析不出来时，会列出候选让你确认。
- 任何失败（网络、权限、找不到仓库）都会给出手动链接，绝不静默跳过。

## 许可证

[MIT](LICENSE)
