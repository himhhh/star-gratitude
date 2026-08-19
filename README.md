# Star Gratitude ⭐

Never forget to star the skills and projects you actually use and enjoy.

**English** · [简体中文](README.zh-CN.md)

A tiny agent skill that reminds you to give a GitHub star to third-party skills and
projects you found useful — because "I should star this later" almost never
happens. It locates the source repo of a skill, checks whether you already
starred it, asks for confirmation, and records every star in a local gratitude
ledger so it never nags you twice about the same repo.

## How it works

The skill triggers in two situations:

1. **First use of a third-party skill** — the first time a session invokes a skill
   from a plugin, `~/.agents/skills`, `~/.dsh/skills`, or a project skills dir,
   it lightly reminds you where the skill came from and asks if you want to star it.
2. **Explicit praise** — when you say a skill or project is good / worth keeping /
   worth starring, it resolves the source repo and walks you through starring it.

In both cases the workflow is the same:

1. **Locate** the source repository (`owner/repo`) from the skill's directory
   (`SKILL.md` / `README.md` / `package.json` `repository` field), `git remote -v`,
   or a web search when nothing is found locally. It never guesses a repo.
2. **Deduplicate** against the local ledger — already starred or already reminded,
   it stops there.
3. **Ask first, star after confirmation** — starring is a public action on *your*
   account, so it always waits for your explicit go-ahead.
4. **Star** with `gh api -X PUT user/starred/OWNER/REPO`.
5. **Record** the star in `~/.dsh/star-gratitude/log.md` (local only, never pushed).

## Installation

Requires the [GitHub CLI](https://cli.github.com/) (`gh`) with `repo` scope
(`gh auth login`).

### DSH / agents using `~/.agents/skills` or `~/.dsh/skills`

```bash
git clone https://github.com/himhhh/star-gratitude.git ~/.agents/skills/star-gratitude
# or
git clone https://github.com/himhhh/star-gratitude.git ~/.dsh/skills/star-gratitude
```

The skill is picked up automatically by the skill watcher — no restart needed.

### Generic

Copy the `SKILL.md` into any directory your agent discovers as a skill root
(e.g. `.agents/skills/`, `.dsh/skills/`, `skills/`).

## Usage

- Say something like *"this skill is really useful, I want to star it"* or
  *"please star that project"*, or simply invoke a third-party skill for the
  first time — the agent will handle the rest, always confirming with you
  before starring.
- The gratitude ledger lives at `~/.dsh/star-gratitude/log.md` and is created on
  first use. Entries are formatted as
  `date | owner/repo | name | why it was useful | status` where status is
  `✅ starred` (done) or `⏳ reminded` (asked, no confirmation — never re-asked).

## Safety rules

- Starring is a **public action** on **your own account** — the agent confirms the
  account with `gh auth status` and always asks before acting.
- Already starred / already reminded → skipped, never nags twice.
- Never guesses a repo: if the source can't be resolved, it shows you candidates
  and lets you confirm.
- Any failure (network, permissions, unresolved repo) results in a manual link,
  never a silent skip.

## License

[MIT](LICENSE)
