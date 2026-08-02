# reddit-read Agent Skill

Read a Reddit post and its comment thread from inside your agent session.

Built because agents (especially Claude Code) can struggle to interact with Reddit, due to the aggressive anti-LLM-scraping policies. This reads it through Reddit's API instead.

## Install

```bash
npx skills add marinsokol5/reddit-read   # Claude Code, Codex, …
npx skills update reddit-read           # later, to pull updates
```

Needs Python 3 and `curl`. Restart your agent session so it picks the skill up.

## Use

```
/reddit-read https://www.reddit.com/r/Hyperhidrosis/comments/1v5nuc9/...
/reddit-read 1v5nuc9
```

## How

An anonymous installed-client token (no account, public data only — nothing a
logged-out visitor can't see), one `GET /comments/<id>`, rendered as markdown
with the comment tree indented. Two requests per thread.

On HTTP 429 it stops and tells you to wait. It never mints a second token to
buy a fresh rate-limit budget.
