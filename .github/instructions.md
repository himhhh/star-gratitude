# 给 AI 的安装与使用说明（GitHub Copilot 指令）

本仓库是一个 **agent skill**（`SKILL.md`），用于提醒用户给「用着好用」的第三方 skill 或项目点 GitHub star。

## 安装流程

前置：已安装 `gh` CLI 并登录（`gh auth login`，需 `repo` scope）。

```bash
# 用户级（推荐，所有项目可用）
git clone https://github.com/himhhh/star-gratitude.git ~/.agents/skills/star-gratitude
# 或 DSH 用户目录
git clone https://github.com/himhhh/star-gratitude.git ~/.dsh/skills/star-gratitude
# 或项目级
git clone https://github.com/himhhh/star-gratitude.git <项目>/.dsh/skills/star-gratitude
```

## 验证

新会话的可用 skill 列表中应出现 `star-gratitude`；否则确认安装目录是 agent 的 skill 根目录。

## 使用约定

- 触发：会话中第一次调用第三方 skill，或用户明确称赞某 skill/项目好用、要求 star。
- 纪律：先提醒、确认后再 star（公开动作）；先查 `~/.dsh/star-gratitude/log.md` 去重；不猜仓库；失败给手动链接。

详见 [`SKILL.md`](SKILL.md)。
