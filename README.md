# SigilGate.github.io

Source repository for the [SigilGate GitHub Pages](https://sigilgate.github.io/) site, built with [Hugo](https://gohugo.io/) and the [Congo](https://github.com/jpanther/congo) theme.

## Branch structure

| Branch | Purpose |
|--------|---------|
| `main` | Content (`content/`), workflows, README, LICENSE |
| `hugo` | Hugo config, theme submodule, static assets |
| `telegram` | Telegram channel content |
| `gh-pages` | Built site (deployed automatically) |

## Content structure

Blog posts and Telegram posts follow a date-based directory layout:

```
content/YYYY/MM/DD/slug/index.md
```

This convention applies to both the blog (`main` branch) and the Telegram channel (`telegram` branch), making it easy to navigate and cross-reference posts across platforms.

## Deployment

### Hugo site

Pushing changes to `content/**` on `main` triggers the `Deploy Hugo Site` workflow:

1. Checks out `main`, overlays config and themes from the `hugo` branch.
2. Builds the site with Hugo (`--minify`).
3. Deploys the result to the `gh-pages` branch.

Use `workflow_dispatch` for manual builds after config changes on the `hugo` branch.

If a new blog post contains the `"new"` tag in its frontmatter, the `Telegram Blog Notification` workflow sends a link to the published page to the [@SigilGate](https://t.me/SigilGate) Telegram channel — as a notification for the community.

### Telegram channel

The `telegram` branch is a standalone content hub for the [@SigilGate](https://t.me/SigilGate) Telegram channel. Pushing changes to `content/**` on this branch triggers the `Publish to Telegram` workflow:

- **New post** — the message is sent to the channel; the `telegram_message_id` field in the frontmatter is populated automatically and committed back.
- **Edited post** — the existing message in the channel is updated via the stored `telegram_message_id`.

Post format:

```yaml
---
title: "Post title"
date: 2026-02-06
telegram_message_id:
---

Post text in Telegram Markdown format.
```

The `telegram_message_id` field must be present and empty in new posts — it serves as the link between the file and the published message. The workflow fills it in automatically after a successful publish.

## License

See [LICENSE](LICENSE) for details.
