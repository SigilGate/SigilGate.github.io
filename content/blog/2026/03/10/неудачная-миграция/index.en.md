---
title: "A Failed Migration"
slug: "a-failed-migration"
date: 2026-03-10T00:00:00+00:00
draft: false
summary: "Unexpected trouble with hosting after migration: servers in Japan suspended without explanation."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["migration", "hosting"]
---

This blog went quiet for a few days, for two reasons. First — **Sigil Gate** is a weekend project, but some weekends are so packed that there's simply no room left for a weekend blog. Second — the project was switching hosting providers. We changed addresses and moved to brand-new servers.

Honestly, I might not have even mentioned it here. The whole move took about an hour, and the migration went nearly seamlessly. That was on the 7th, and on the 8th and most of the 9th everything ran almost flawlessly.

However, toward the end of this long weekend I logged into the hosting control panel and found the servers suspended — with no explanation given.

I don't know, at this point, what happened: the hosting services are paid six months in advance, so it shouldn't be a billing issue. There may be some loose ends left over from the instances we migrated away from. I also suspect that in my last session I was being rather aggressive with security hardening and may have overdone it somewhere. I put strict rules in place, significantly altered sshd's behavior — and the hosting provider may have found that somewhat suspicious. More on the new security requirements in the documentation section [Security/SSH CA]().

Either way — for now there's nothing to do but wait for support to respond. It looks like I may soon be swearing in Japanese, because they respond to plain Japanese very slowly.

Fingers crossed — these are the core network servers, located in Japan. The edge servers located in Russia are still running, but without the Japanese servers they're only good for serving catgirl websites.
