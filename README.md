# SigilGate Telegram Content

Content hub for the [@SigilGate](https://t.me/SigilGate) Telegram channel.

## How it works

- Add a post to `content/YYYY/MM/DD/slug/index.md` — it gets published to the channel automatically.
- Edit an existing post — the message in the channel gets updated.

## Post format

```yaml
---
title: "Post title"
date: 2026-02-06
telegram_message_id:
---

Post text in Telegram Markdown format.
```

The `telegram_message_id` field is populated automatically after publishing.
