# AGENTS.md — 给 AI Agent 的安装与使用说明

本仓库是一个 **agent skill**（`SKILL.md`），用于提醒用户给「用着好用」的第三方 skill 或项目点 GitHub star。

## 安装（在目标机器上执行）

前置条件：已安装 [GitHub CLI](https://cli.github.com/)（`gh`）并登录（`gh auth login`，需 `repo` scope）。

```bash
# 方式一：安装到用户级 skill 目录（推荐，所有项目可用）
git clone https://github.com/himhhh/star-gratitude.git ~/.agents/skills/star-gratitude

# 方式二：安装到 DSH 用户目录
git clone https://github.com/himhhh/star-gratitude.git ~/.dsh/skills/star-gratitude

# 方式三：项目级（仅当前项目可用）
git clone https://github.com/himhhh/star-gratitude.git <项目>/.dsh/skills/star-gratitude
```

## 验证安装

确认 skill 被 agent 的 skill watcher 发现：新会话的可用 skill 列表（`<available_skills>` 目录）中应出现 `star-gratitude`。若未出现，检查目标目录是否为 agent 配置的 skill 根目录。

## 使用方式（agent 行为约定）

触发时机（仅在以下情况，不要主动打扰）：

1. 会话中**第一次**调用某个第三方 skill（来自插件、`~/.agents/skills`、`~/.dsh/skills` 或项目 skills 目录）时，按 `SKILL.md` 的「工作流 A」轻量提醒一次；
2. 用户**明确称赞**某个 skill/项目好用或要求 star 时，按「工作流 B」处理。

核心纪律：

- **先提醒、确认后再 star**：star 是用户账号上的公开动作，永远先征求明确同意，同一轮内不得执行。
- **查重**：先查 `~/.dsh/star-gratitude/log.md`（账本），已 `✅ starred` 或 `⏳ reminded` 的来源不再重复提醒。
- **不猜仓库**：来源解析不出来时，给出候选链接让用户确认。
- 依赖 `gh` 与 `gh auth status` 确认账号；失败时给出手动链接，不静默跳过。

完整流程见 [`SKILL.md`](SKILL.md)。
