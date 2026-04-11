---
title: "Picking Up the Pace!"
slug: "picking-up-the-pace"
date: 2026-02-16T12:00:00+00:00
draft: false
summary: "Automating user and device management, changes to the data storage system, and the idea of trial access via a Telegram bot."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["automation", "scripts"]
---

Riding the high from [yesterday's progress]({{< ref "blog/2026/02/15/new-device" >}}), I decided to keep the momentum going — and continue the automation theme.

I wanted to close out a logical milestone for our scripting system. Yesterday we learned how to add devices — but that's just one operation from the classic set. The standard operations for this kind of system — **create, delete, modify** — for both users and their devices.

## A Small Change with Big Implications

Along the way, I made a small change to the storage system: added **Telegram ID** to user properties, alongside their Telegram username. Seems like just one field. But I'm buzzing with the ideas behind it. Let me explain.

As you know, I'm planning to expand our network to several dozen, maybe even hundreds of nodes — and gradually move it toward, if not full monetization, at least **breaking even**. And the trickiest part here is probably attracting new users. In a way that feels organic and natural.

I want to reach a growth stage where I can test many things not on two or three nodes, but on at least **three internal and nine external nodes**. That scale is sufficient to battle-test techniques that will later work on several hundred nodes just as well as they did on a few. Unfortunately, prototype scale doesn't allow for that. And to reach a larger scale — you need to solve the financial equation and attract new users.

Word of mouth and future social media activity could certainly get things moving. But I'm afraid the conversion rate won't be very high. It's one thing to hear "there's this thing that works," and quite another to **touch it, feel it, try it out**.

## Here's an Idea!

And so, against the backdrop of a morning discussion about Telegram being down again and everything lagging once more, an idea hit me — quite simple, but no less brilliant for it: **what if we launched a trial version?** Available to everyone. Say, through Telegram.

The mechanics could be dead simple. Create a bot with a single button — **"Get Trial Access."** On click, a new device is created that operates on the network for about an hour, then gets deleted. The number of allowed trial keys can vary — from three to ten. After that, a polite message appears: you've exhausted your trial access limit, come join us (for a fee, of course)! User uniqueness is tracked by **Telegram ID** — and that's exactly why I needed that field.

The user gets a free connection and a few hours of unrestricted internet to test the service. We get new users if the model works. Subscription management, status, personal dashboard — all of that can be packed into the bot's functionality later.

## For Now — Groundwork

In the excitement of my own brilliance, I immediately created a **trial** user, to which I'm now ready — using the scripts we've already written — to add, delete, and modify devices. **Fully automated** — the logic for this functionality is already taking shape in my head.
