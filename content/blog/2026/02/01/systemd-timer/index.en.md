---
title: "systemd timer: time to leave the cave"
date: 2026-02-01T20:00:00+00:00
draft: false
summary: "I've been using cron my whole life. Turns out there's been something better for quite a while."
tags: ["linux", "systemd", "automation"]
---

I feel like a caveman.

I've been around for a while, and there are things I've grown so accustomed to that I treat them as constants. They're always in their place. They always work. I never wonder if there's something else — because why would I, if everything's fine as it is?

When I need to run something on a regular schedule — I use cron. I don't know how old this utility is. It feels like it's been around forever. Open `crontab -e`, write a schedule, forget about it. It works.

But today, while solving yet another periodic task, I was surprised to discover that an alternative exists. And has existed for quite some time.

## systemd timer

systemd timer is a task scheduling mechanism built into systemd. It solves the same problems as cron, but with greater precision and more options:

- **Second-level granularity.** Cron's minimum interval is one minute. systemd timer has no such limitation.
- **No duplicate runs.** If the previous run hasn't finished yet, a new one won't start. Cron doesn't control this: if a script runs longer than the interval, a second instance launches on top of the first.
- **Logging via journalctl.** All runs, outputs, errors — in one place. No need to manually redirect stdout to a file.
- **RandomizedDelaySec.** Random delay for the start time. The task doesn't fire exactly on schedule — it starts at a random moment within a defined window. This makes timing patterns harder to detect — useful when predictability is undesirable.
- **Dependencies.** You can specify that a task should only run after the network is up, after another service has started, after a disk is mounted. Cron just fires on schedule, regardless of system state.
- **Persistent.** If the system was off when a scheduled run was due, the task runs immediately after boot. Cron simply loses missed runs.

## How I used it

Recently I needed to set up automatic rotation of randomly generated gRPC paths between nodes. The task runs every hour — but not exactly at :00. It starts with a random offset of up to five minutes. If the previous rotation took too long (say, an SSH connection hung) — the next one won't launch on top of it.

I could have used cron for this. But cron can't do random delays or duplicate protection. I'd have to build a wrapper: `flock`, manual jitter via `sleep $((RANDOM % 300))`, log redirection to a file...

I think it's time to leave the cave.

## What's next

Now that I know about systemd timer, here's what's on the agenda:

- **Set up monitoring via journalctl** — alerts on rotation errors, log aggregation.
- **Fine-tune the random delay** — to make statistical analysis of rotation timing even harder for detection systems.
- **Network dependency** — don't attempt rotation if the network hasn't come up yet after a reboot.
- And possibly more things that systemd timer can do and cron can't. I suspect I've only scratched the surface.
