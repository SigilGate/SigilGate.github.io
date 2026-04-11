---
title: "Connected"
slug: "connected"
date: 2026-03-03T00:00:00+00:00
draft: false
summary: "The support system I'd been dreaming of since February is finally built — in a single evening. Tickets, bot-mediated dialogue, handoffs between admins, and broadcasts."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["бот", "телеграм", "автоматизация"]
---

We had plenty of things we didn't have at all. Including a way for users to reach us. The infrastructure grows, automation hums along, the bot hands out configs, takes requests, manages devices. But a simple, human way to write in with a question and actually get a reply — that didn't exist.

It had been bothering me for a while. [Back in February I mentioned it in passing]({{< ref "blog/2026/02/22/как-развивать-проект-не-накапливая-технический-долг" >}}) — one of those ideas you want to build but keep pushing aside for something more urgent. The kind of "small thing that won't take long." And that's exactly the kind of thing where history proves: you either put it off forever, or you just sit down and do it.

Last night I sat down — and did it.

## How it works

The architecture is simple but complete. No external services — everything goes through the bot.

**A user** can open a support request in two ways: via the "Write to admin" button in the main menu, or via "Report a problem" right from a device card — if the issue is specific. Each request gets a status, an identifier, and a creation timestamp. Through `/appeals`, users can see their history: open and resolved.

From there, a **dialogue** begins — and here's the key detail: user and admin communicate through the bot. The admin's contact details stay hidden. No personal data, no "just DM me." An official support channel is open.

**The admin** gets notified about a new request and can: take it, reply to the user, hand it off to another admin, or close it. On handoff, all admins get notified — nothing falls through the cracks.

## Broadcasts

Alongside that, a `/send` command appeared for admins — a flexible broadcast tool. Four modes:

- **Channel** — post to the project's Telegram channel as the bot.
- **Broadcast** — send to all active and inactive network users.
- **User** — a notification to a specific user.
- **Everyone** — channel and all users at once.

The flow is dead simple: pick a target, write the message, preview it, confirm — and the bot delivers.

## One more small thing

While I was at it, I cleaned up the last remaining text commands from the bot's menus. Now both users and admins have only buttons. Three for the user: devices, requests, write to admin. Three for the admin: users, requests, send message. Clean and pleasant.

---

Putting this off any longer wasn't an option. Not because it was urgent — but because without it, the project felt a little lopsided: a user could join the network just fine, but had no way to ask for help. That's fixed now.
