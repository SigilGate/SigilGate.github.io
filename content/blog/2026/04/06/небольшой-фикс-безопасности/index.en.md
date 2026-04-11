---
title: "A Small Security Fix"
slug: "небольшой-фикс-безопасности"
date: 2026-04-06T00:00:00+00:00
draft: false
summary: "Someone hinted that storing Telegram IDs in plaintext is not a great idea. I fixed it. Here's how — and why it turned out trickier than expected."
categories: ["Дневник проекта"]
rubrics: ["Хроники проекта"]
tags: ["безопасность", "бот"]
---

Someone recently dropped a hint in conversation: our database stores sensitive user data. Telegram ID, to be specific — the same one we [collect at registration]({{< ref "blog/2026/02/18/telegram-bot" >}}).

One might ask — so what? The registry is private, the repository is private. But the probability of a leak is never zero. And storing in plaintext what you don't have to store in plaintext is simply bad practice. I promised to fix it at the first opportunity. I kept my word.

## What Changed

Previously, every user record contained a `telegram_id` field — a plain integer. That field is gone now. In its place, two new fields:

- **`hash_telegram_id`** — an irreversible hash (HMAC-SHA256). The system uses it to look up users on every bot interaction.
- **`encrypted_telegram_id`** — an encrypted Telegram ID (symmetric Fernet encryption). Used wherever a message needs to be sent to the user.

The same applies to the support system: `telegram_id` and `admin_telegram_id` fields in tickets have been replaced with their encrypted counterparts. Trial device naming also changed: device names used to start with the user's Telegram ID, now they start with a hash prefix.

## Where It Got Tricky

At first glance it seemed simple: take `telegram_id`, compute a hash, swap it out — done. But then I discovered that this field lives in two completely different worlds.

**World one** — identification and management. Who are you? What's your role? Which devices are yours? A hash is sufficient here. It's irreversible — even knowing the hash, you can't extract the Telegram ID from it. Problem solved.

**World two** — live communication. [Approval notifications]({{< ref "blog/2026/02/18/telegram-bot" >}}), [support ticket replies]({{< ref "blog/2026/03/03/диалог-налажен" >}}), [broadcasts]({{< ref "blog/2026/03/03/диалог-налажен" >}}). Here the bot sends messages directly through the Telegram API — and the API only accepts real numeric IDs. A hash is of no use whatsoever.

This is where the problem lies. Using a hash instead of an ID for sending is impossible by definition. So the options are: store the real ID (but not in plaintext), or obtain it at send time without touching the database.

## The Interim Solution

I went with the pragmatic option: encrypt the ID with a symmetric key. Now the database holds an opaque token instead of `358669266`. The bot decrypts it at send time and works exactly as before.

It's not perfect. The encryption key lives in the bot's environment — meaning to get to the real IDs, you'd need to compromise the server itself. That's significantly harder than just reading a JSON file from a leaked repository — but it's not «impossible».

I consider this an interim solution. Acceptable — but temporary.

## Where This Is Headed

If you've read [«Terms of Entry»]({{< ref "blog/2026/04/05/условия-входа" >}}), you'll remember: the next major milestone is a **distributed data store**. At that point I'll be rewriting the bot from scratch. And that's likely when the final solution to this problem will emerge.

The idea: the bot works with members of a Telegram group or channel. When a message needs to be sent to a user, the bot iterates through members, computes a hash for each, and looks for a match. Found one — sends the message. The data store only ever holds a hash. No encrypted IDs, no decryption keys. Sensitive data never leaves the bot's memory.

The idea is sound, but it needs careful implementation — and, more importantly, the right architectural foundation. That doesn't exist yet. But there's a promise in place — and as you can see, I keep them.

---

Good occasion to thank whoever noticed. Sometimes an outside perspective catches what you keep walking past without looking.
