---
name: folo-reader
description: Use when the user asks to read RSS articles, manage subscriptions, search feeds, check unread counts, import/export OPML, save articles, or generate daily briefings via Folo. Wraps the `folo` CLI (npm package folocli by RSSNext/diygod).
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [folo, rss, reader, feed, subscription]
    related_skills: [wechat-article-pipeline]
---

# Folo Reader

## Overview

Folo (formerly Follow) is an open-source RSS reader by the RSSNext team. Its CLI tool `folocli` lets you read articles, manage subscriptions, search feeds, and export OPML directly from the terminal. All output is JSON, making it ideal for scripting and AI agent integration.

This skill wraps the most common `folo` CLI operations. Just tell me what you want to do — I'll translate it into the right command.

## When to Use

- User asks "what's in my unread", "read my RSS feeds"
- User wants to search for feeds on a topic
- User wants to add/remove/manage subscriptions
- User wants to export a backup OPML or import from OPML
- User wants to see the latest articles from a specific feed
- User wants to generate a daily/weekly reading brief

**Don't use for:**

- User wants to read WeChat Official Account articles → use `wechat-article-search`
- User wants to publish an article to WeChat → use `wechat-article-pipeline` or `wechat-publisher`
- User just wants to browse the web → use `web_search` / `web_extract`

## Quick Start

All commands use the `folo` binary (pre-authenticated, token at `~/.folo/config.json`).

```bash
folio timeline --unread-only --limit 20   # view unread articles
folio subscription list                    # list subscriptions
folio entry get <entryId>                 # article detail (full body)
folio entry read <entryId>                # reader-mode extraction
```

### View Unread Articles

```bash
folio timeline --unread-only --limit 10
folio entry get <entryId>       # detail
folio entry read <entryId>      # readability extract
folio entry mark-read <id>      # mark as read
```

### Manage Subscriptions

```bash
folio search discover <keyword>           # search for feeds
folio subscription add --feed <rss_url>   # add a feed
```

### Pagination

Timeline responds with `hasNext` + `nextCursor`:

```bash
folio timeline --limit 20                              # page 1
folio timeline --limit 20 --cursor <nextCursor>         # page 2
```

### OPML Import/Export

```bash
folio opml export --output my.feeds.opml
folio opml import my.feeds.opml
```

### Search Trending

```bash
folio search trending --range 1d --limit 10 --language cmn   # trending in Chinese
folio search trending --range 7d --limit 10                    # this week's trending
```

### Generate Briefing

```bash
# Fetch latest unread, extract body per entry, aggregate as Markdown brief
folio timeline --unread-only --limit 20 --format json | jq ...
```

## Common Pitfalls

1. **folocli not installed.** If the `folo` command is missing, install it first: `npm install -g folocli`. Then verify with `folo --version`.

2. **Using `npx` instead of calling `folo` directly.** The CLI is globally available after install — use `folo` directly, never `npx`.

3. **`entry get` returns raw HTML.** The `content` field contains full HTML. Strip tags before displaying, or use `entry read` instead. `entry read` returns reader-mode extracted text (cleaner, but may be empty for some feeds).

4. **`UNAUTHORIZED` error.** The token has expired. Run `folo login` in the terminal to re-authenticate (browser OAuth flow).

5. **Assuming one page is everything.** Timeline returns a limited batch by default. Check the `hasNext` field and paginate with `nextCursor`. Don't assume one call gets everything.

6. **Forgetting to parse JSON.** All output defaults to JSON format. Use `json.loads()` in Python — don't treat it as plain text.

## Verification Checklist

- [ ] `which folo` finds the command, or `folo --version` prints a version (confirm installed)
- [ ] `npm list -g folocli` confirms the global package is installed
- [ ] `folio whoami` returns user info (confirm logged in)
- [ ] `folio subscription list` lists subscriptions
- [ ] `folio timeline --unread-only --limit 5` returns articles
- [ ] Token file `~/.folo/config.json` exists and is non-empty

## Reference

Full command reference at [`references/commands.md`](references/commands.md), covering auth, timeline, entry, subscription, search, feed, lists, collections, OPML, unread, error recovery, and environment configuration.
