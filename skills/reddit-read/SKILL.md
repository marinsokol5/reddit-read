---
name: reddit-read
description: Read a Reddit post and its full comment thread as markdown, from a URL or post id. Use whenever the user runs /reddit-read, pastes a reddit.com link, or asks you to read, summarize, or discuss a Reddit thread — fetching reddit.com directly is blocked, this is the way in.
---

# reddit-read

The bundled script `scripts/reddit-read` (beside this file) prints one thread —
post plus its nested comments — as markdown on stdout.

```bash
python3 <this skill's base directory>/scripts/reddit-read "<url-or-post-id>"
```

- **It needs network access to `reddit.com`.** That is not in the default
  sandbox allowlist, so either add `reddit.com` and `*.reddit.com` to
  `sandbox.network.allowedDomains` in settings (once, preferred), or run the
  command with the sandbox disabled. A connection error means neither is set.
- One argument, no flags. Takes a full URL, a mobile `/s/` share link,
  `redd.it/xxx`, `t3_xxx`, or a bare post id.
- A big thread takes up to about a minute (comment pages are paced) and is
  capped; the output's last line says how many reply stubs were left unfetched.
- Errors are one line on stderr with exit 1: `rate limited` (wait a minute and
  retry), `no such post` (deleted, private, or quarantined),
  `no Reddit post id found`, or `blocked by Reddit's network security` (Reddit
  changed its blocking again; the script needs updating, retrying won't help).

Read the output, then answer what the user actually asked about the thread —
don't echo the whole thing back unless they want it.
