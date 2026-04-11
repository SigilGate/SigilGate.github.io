---
title: "Terms of Entry"
slug: "terms-of-entry"
date: 2026-04-05T00:00:00+00:00
draft: false
summary: "The project is still closed — but I've started bringing in new participants. Here's how the collaboration works and where it's all heading."
categories: ["Дневник проекта"]
rubrics: ["Хроники проекта"]
tags: ["архитектура", "etcd", "участники"]
---

The project is still closed. But since April I've started moving it into the open — and have even accepted a few new participants. I don't connect just anyone to the network — applications from people I don't know I generally decline. This is a matter of principle: I'm building a small private network, not a public-facing service.

There are concrete reasons for this — and in this post I want to talk openly about the terms of joining and why I'm expanding at all.

---

In March, we lost part of the infrastructure. Before that, the project ran on what we had — and it was mostly enough. But when you start thinking about resilience, you realize: one or two servers isn't enough. You need nodes. And not peripheral ones — those I have no problem with — but core nodes.

The core is what sits behind the perimeter. Foreign hosting. And here you run into the same story every time: crypto, dollars, tenge, non-standard payment schemes, a new workaround each time. All of it is solvable — but it wears you down. So I started looking for people who have the means to spin up core servers and are willing to hand them over to my operational control.

---

Here's how it works in practice.

A new participant independently sets up a cluster of Entry and Core nodes. The server stays in their ownership: they pay for it, they monitor its health, they are responsible for its traffic with the hoster. I take it under operational control — gaining access to deploy shared network infrastructure on top of it. The owner remains the sole user of their node. I may connect occasionally — but only to use the node as a reserve, not as a constant load.

Anyone who decides to leave the project for any reason can simply shut down their servers at the hoster. At any time, the final decision stays with them.

The arrangement suits me for another reason as well: each owner independently monitors their node and pays their hoster bills on time. A healthchecking and load monitoring system is on the roadmap — when exactly it gets built is another question.

---

Now about the architecture — for general context.

The network is built on a core-periphery model. Core nodes are powerful, permanent servers that hold everything else together. The periphery is a shell: less powerful machines, more fluid, with planned rotation of IP addresses. Each core node has its own isolated periphery attached to it — a separate cluster that doesn't overlap with the others.

My goal is to make deploying and tearing down the periphery painless. Add a node, remove it, swap it — automatically, with minimal effort. The address pool needs to turn over on a regular basis: **persistence is a target**, as I've written before.

And here comes the next level of the problem. The whole system needs shared distributed data storage — not centralized. I'm planning to build it on **etcd**, with an API for working with the data on top of it, and then user-facing interfaces: Telegram, web. The general idea: any request is handled from any core node, and the network survives the loss of some of them without pain.

---

Looking further ahead.

I want every participant to have their own full cluster — one they monitor and maintain themselves. And for emergencies — the ability to exit through someone else's node and rebuild everything from scratch, like a phoenix from the ashes. That's exactly what the shared storage is for: not as a control center, but as a distributed foundation that lives across all nodes at once.

That's the end goal. Each segment is independent, but if one is compromised — the others survive. And every participant knows there's a way out.

---

Ultimately, this project is more social than commercial for me — at least at this stage. I'm building it to serve my own goals: a resilient infrastructure for unrestricted access to the internet, for using services, for communicating and exchanging information without friction.

I also use it for self-promotion and building a personal brand — running the blog, social media, publishing on specialized platforms — all of which takes up about 80% of the time I could otherwise spend on the technical side of the project. I have to talk to people — not because I have an outsized ego or a craving for likes. All of this is well outside my comfort zone.

But even dinosaurs like me have to adapt to survive the evolution.
