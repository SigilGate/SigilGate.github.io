---
title: "The Telegram Bot"
slug: "telegram-bot"
date: 2026-02-18T00:00:00+00:00
draft: false
summary: "The project bot gets a new life: from a technical publishing tool to a full-featured interface for network users."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["automation", "bot", "telegram"]
---

Life is like a zebra. After a black stripe comes a white one. Then black again, white, black, white... and at the very end, as tradition demands, the tail end. After a somewhat stressful stretch at my day job, I suddenly discovered something rare and precious: **free time**. Time for me, when I can do absolutely **nothing**. And for me, that means throwing myself into the things I love with twice the energy!

I didn't let my [brilliant trial access idea]({{< ref "blog/2026/02/16/picking-up-the-pace" >}}) gather dust — and jumped straight to making it happen. Or rather, to its necessary prerequisite: **the Telegram bot**.

## The Bot We Deserve

The project already had a bot. It appeared at the very beginning — but was used exclusively as a Telegram API endpoint: publishing posts to the channel and pretending to be nothing more. A technical worker on minimum wage, doing the same thing over and over again.

Time to give it a promotion.

## Why the Bot Needs New Powers

I spent the previous week on automation — [writing bash scripts]({{< ref "blog/2026/02/15/new-device" >}}) that cover all the main user and device management scenarios. Full CRUD, clean commit history, no manual fiddling with configs.

But there's a problem: typing commands into a black terminal screen is an admin's job. And an admin is a finite resource. A lot of what users could do on their own currently requires my involvement: create a device, delete it, get the client config. My long-term plan involves a web panel with a personal dashboard. But for now, a Telegram bot with a limited command set will do — a fast, practical solution that covers the main use cases.

## Architecture

The solution is dead simple. The bot is **middleware**: it receives commands and runs the already-written scripts from the [scripts](https://github.com/SigilGate/scripts) repository in a separate process. No new logic — just a new interface on top of the existing automation.

User identification is by Telegram ID. (Which, by the way, is exactly why I [added that field]({{< ref "blog/2026/02/16/picking-up-the-pace" >}}) last time.) Depending on the user's role in the system, the available actions differ:

- **Unregistered user** — submit a connection request or ask for trial access.
- **Registered user** — manage their own devices: create, delete, get configuration.
- **Administrator** — full access: user management, statuses, network state, and everything else.

I decided to put the trial access mechanics on the back burner — there are too many details I want to think through properly rather than on the fly. But the architecture is already in place, and the trial will fit naturally into it when the time comes.

## Already Live

The minimum viable implementation is up and running. The bot is available — you can start using it right now.
