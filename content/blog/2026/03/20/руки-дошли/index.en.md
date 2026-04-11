---
title: "Finally Got Around to It"
slug: "finally-got-around-to-it"
date: 2026-03-20T00:00:00+00:00
draft: false
summary: "The Root node has been running for five days. Today I brought up the supporting infrastructure: the admin bot and the subscription microservice — both containerized, with auto-deploy."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["восстановление", "инфраструктура"]
---

Five days ago I wrote that [connection had been restored]({{< ref "blog/2026/03/15/связь-восстановлена" >}}), and immediately added: "I'll connect the bot and other services later — the network is up already." Later turned out to be today.

The work week is structured in a way that leaves almost no room for the project. So the network has been running in a minimal configuration: only what was absolutely essential. Everything else — later.

---

Today I deployed the supporting infrastructure.

The **admin bot** is the tool administrators use to manage the network: register and disable users, manage their devices. Without it, administration becomes manual console work — possible, but tedious. Along the way I packaged the bot into a Docker container: a one-time investment that eliminates the need to configure dependencies from scratch on every new node. Move to a new server — clone, start the container, done. GitHub Actions is set up so that every push with code changes automatically rebuilds and restarts the container.

The **subscription microservice** is a more interesting piece. Users don't receive direct node addresses — they get a subscription link, a dynamic list of configurations that updates on the server. When we move to permanent hosting, clients won't need to change anything on their end: update the subscription and the connections switch over automatically. The service was already containerized in advance, so deployment was nearly effortless.

---

The Root node remains what it shouldn't be: it knows everything and holds everything. This is forced, it's temporary, and I'm keeping that in mind. But now, at least, it holds not just the network — but all the tooling that serves it.

The [search for permanent hosting]({{< ref "blog/2026/03/19/стратегия-второго-эшелона" >}}) continues.
