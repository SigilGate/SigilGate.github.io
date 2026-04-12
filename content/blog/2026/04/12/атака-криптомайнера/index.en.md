---
title: "Cryptominer Attack"
slug: "cryptominer-attack"
date: 2026-04-12T00:00:00+00:00
draft: false
summary: "How I got hacked, had my CI/CD taken down, and had tokens vacuumed across the entire core cluster — in fifteen minutes."
categories: ["Архитектура и исследования"]
rubrics: ["Защитные руны"]
tags: ["new", "безопасность", "инцидент", "github", "инфраструктура"]
---

Last week I got hacked. Not that I lost much sleep over it — but the episode had consequences.

I ran a small investigation. I was able to reconstruct the timeline almost minute by minute. A few gaps remained — but they fill in logically. Here's what I found.

## First Signal

It started with an anomaly in my GitHub account: repositories I hadn't created. Random names, strange contents. A quick look at the code — mining, cryptocurrency, Monero.

That was enough to make everything immediately clear. Okay, I've been hacked. What now?

Rule number one from The Hitchhiker's Guide to the Galaxy: "DON'T PANIC!!!" Open settings. Thank heavens, the account isn't locked. Revoke all access tokens, change the password, restart two-factor authentication. Then clean up the artifacts — they'll still be available on GitHub's servers for about a month for forensic purposes, but there's no reason to keep material on your own account that could be used for an attack from within.

## Damage Assessment

Next step — check all repositories, review recent commits. All clear here: the attack went in a different direction, data untouched. Deep breath.

Check the logs. The entire attack took about **fifteen minutes**. In that time, a dozen repositories were created under my name — three commits each. Same pattern, minimal time spread. Obviously a bot: no wasted moves, straight to the target by script.

The target — **GitHub Actions** servers. Repositories with mining code were supposed to spin up in my workers and start mining Monero at someone else's expense.

The good news: GitHub Actions shut the attacks down at the root. No lasting consequences, minimal free tier usage, I didn't go into the red. Phew.

The bad news: noticing the suspicious activity, GitHub restricted my access to the service. The entire CI/CD pipeline — builds, deploys, everything — came to a dead stop from the moment of the attack. And the attack, as these things go, happened in the middle of the night, close to early morning. While everyone was asleep.

## Where the Leak Came From

The most intriguing question: how exactly did my credentials leak?

I check the network infrastructure. All servers, all logs, all activity. Stop.

On one of the core nodes — a file with a date and timestamp matching the start of the attack. Check the contents: **GitHub credentials**. There it is. The file was "vacuumed": the GitHub access token was extracted, then overwritten with an empty value.

I check similar files on other core servers. Everywhere except the root node — same story, a few minutes apart. Someone methodically worked through every machine that trusted each other and allowed passwordless login. Grabbed the personal token — the one I used to send data backups to a private repository. You can guess the rest.

## What This Means

The backup token was stored in plain text in `~/.git-credentials`. A simple, convenient solution for automation. And completely unreliable, as it turned out.

The core servers were joined in a trusted network: SSH key-based login everywhere, no passwords, all core servers trusting each other. Convenient for cluster management. But for an attacker who gains access to one machine, the rest is just a matter of technique. Break in at one point — the whole cluster is compromised.

The personal token had excessive permissions: it was needed to write to one private repository, but technically opened up much more. Which is exactly what was used.

## Digging Deeper

Beyond this — it's all guesswork.

Where the breach happened — unknown. A quick scan found nothing suspicious: no obvious signs of infection, no process anomalies. But security was lax in places. One machine had password-based SSH login still open — brute force is possible.

There's another theory. I don't rotate keys often. And a few months ago we lost part of the infrastructure — and I parted with those servers ungentlemanly: access disappeared before I had time to turn their contents to digital ash. The leak could have come from there.

Other scenarios exist too — I was never paranoid about security. Maybe it's time to start.

## Conclusion

Attack complete, damage assessed.

Personal GitHub account — compromised, recovered. The network infrastructure was largely unaffected and continues to operate — a glancing blow. But the entire core cluster is fully compromised. What was convenient for operational management is completely unsuitable for scenarios like this.

Time to think about architecture from a security standpoint. Time to regroup.
