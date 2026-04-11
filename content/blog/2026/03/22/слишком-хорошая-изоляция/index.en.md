---
title: "Too Good an Isolation"
slug: "too-good-an-isolation"
date: 2026-03-22T00:00:00+00:00
draft: false
summary: "Infrastructure up, bot running — and then it turns out it can't do anything. The cost of overly tidy containerization."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["инфраструктура", "docker"]
---

[Last time]({{< ref "blog/2026/03/20/руки-дошли" >}}) I cheerfully reported: all supporting infrastructure is up, bots are running, containers build automatically. Victory. Curtain.

The curtain came up too early.

---

It turned out the admin bot works in exactly one sense: it's running. It responds to commands, can send notifications — alive, in short. But everything involving file operations fails. Add a device — fails. Generate a connection config — fails. Trial access — also unavailable.

The cause, I think, is obvious: I packaged the bot into a Docker container and isolated it too well. Inside the container — a clean environment with all dependencies. Outside — the data the bot can no longer reach. Everything it needs to read and write lives on the host, and I didn't bother mounting the right directories via `volumes`. The result was predictable.

Fixing it is probably not hard. Maybe even easier than writing this post. But my head is elsewhere right now.

---

Over the past few days I've been unexpectedly seized by a passion for tidiness. Sorting through git: organizing folders, reviving old abandoned projects, sprucing up my personal page. Somewhere last autumn, when the VPN suddenly went down, I dropped another project — now I want to pick it back up. Useful busywork, but nothing to do with the bot.

So the bot remains in a nearly-working state for now. Infrastructure exists, functionality doesn't. When it's fixed — I'll report back.
