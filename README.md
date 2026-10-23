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
# URL or an ID
/reddit-read https://www.reddit.com/r/Hyperhidrosis/comments/1v5nuc9
/reddit-read 1v5nuc9
```

## How

The post comes from the API: an anonymous `installed_client` token, then
`GET /by_id/t3_<id>`. The token has no account behind it, reads only public
data, and shows nothing a logged-out visitor can't see. It authenticates with
the Reddit **Android app's client id**, which is a *public* OAuth client: the id
ships inside the APK and the Basic auth password is empty. It does mean these
requests identify as the Reddit app rather than as you.

The comments come from the web. Since 2026-10-20 Reddit blocks logged-out API
reads of comment trees ("You've been blocked by network security"), so the
script reads the HTML partials that reddit.com itself loads for logged-out
visitors: `/svc/shreddit/comments/r/<sub>/t3_<id>`, 25 comments at a time, plus
the "more replies" stubs (POST the cursor back). It paces those requests, caps
them at 40 per thread, and renders everything as markdown with the comment tree
indented. Credit: [jampick/lurk#54](https://github.com/jampick/lurk/issues/54);
background: [redlib-org/redlib#591](https://github.com/redlib-org/redlib/issues/591).

If Reddit blocks that too, it falls back to the thread's RSS feed
(`/comments/<id>/.rss`): up to ~500 comments in thread order, but flat, with no
nesting and no scores. The output says when it did.

On HTTP 429 it stops and tells you to wait. It never mints a second token to
buy a fresh rate-limit budget.

## License

MIT — see [LICENSE](LICENSE).
