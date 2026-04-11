---
title: "Trial Is Live"
slug: "trial-launched"
date: 2026-02-20T12:00:00+00:00
draft: false
summary: "The /trial command is up and running: a guest gets a link, a QR code, and an hour of open internet. Here's how it works under the hood."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["бот", "триал", "автоматизация"]
---

[Last time]({{< ref "blog/2026/02/16/picking-up-the-pace" >}}) I solemnly announced my "fairly simple yet no less brilliant" idea — trial access via Telegram bot. Then [wrote the bot itself]({{< ref "blog/2026/02/18/telegram-bot" >}}). And then spent a good while thinking about how to make the whole thing work without crutches and unnecessary dependencies — no database, no state files, nothing at all.

Sat down. Thought it through. Implemented it.

Telling myself "done" — that's a rare and sweet feeling.

## How the BOT Works

An unregistered user sends the bot `/trial` — and a few seconds later receives:

- A **VLESS link** to connect to the **Sigil Gate** network
- A **QR code** — to scan with a phone when setting up another device
- A **"Copy link" button** — because manually selecting a 300-character string is already a clinical condition

The link lives for **one hour**. After that, the device is deactivated. The user gets **ten** such attempts. After that, the bot politely hints that it might be time to register.

## The Clever Bit Inside

What I like most is how the limit is tracked — without a single extra table. All state is encoded directly **in the device name**.

Each trial device is named `{telegram_id}{digit}` — and the digit at the end is the remaining limit. The first device for a user with ID 123456789 gets the name `1234567899`. The next one — `1234567898`. Once we hit zero — the limit is exhausted. The bot can check the state at any moment with a single query to the registry — it searches for devices by prefix and looks at the minimum digit. The registry is the only "database".

Things like this bring disproportionately large satisfaction relative to their technical complexity.

## Cleaning Up After Itself

An hour passes — the device needs to be deactivated and removed from Entry nodes. On the Core node, a **systemd timer** runs a cleanup script every hour. It finds all expired trial devices — using file mtime in the registry, since the `created` field only stores the date without time — deactivates them and removes them from the nodes. It also prunes archived records, keeping only the one needed for the counter.

The bot doesn't wait for the timer either: on every `/trial` call, it **lazily checks** the user's active devices and sends expired ones to deactivation in the background.

## QR Code. Just a QR Code.

I have to stop on this separately.

When I was adding the "Copy link" button, the thought flashed: "A QR code would be an absolute luxury." The luxury took about fifteen minutes to implement. The `qrcode` library does everything on its own, and `aiogram` can accept an image directly from an in-memory buffer — no file on disk needed. Scan with your phone — you're connected. It's so convenient that I'm now a little embarrassed I didn't do it from the start.

Yes, we've brought **another dependency** into the project. But it was worth it.

Now we have a convenient interface that lets any user — even an unregistered one — connect to the **Sigil Gate** network and get access to the open internet.
