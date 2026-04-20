---
title: "New Tenants"
slug: "new-tenants"
date: 2026-04-20T12:00:00+00:00
draft: false
summary: "Two empty clusters waited in the cold for their first tenants: data moved into etcd, and k3s welcomed its first service — a database API built on FastAPI."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["kubernetes", "fastapi", "etcd", "api", "архитектура-2.0"]
---

For some time, two empty clusters drifted alone somewhere in the middle of the endless Universe, in the cold, lifeless vacuum of space. This went on for an indeterminate period — two days, actually — until the first living cells began to emerge from the primordial soup of our project. From these cells, a fully functioning organism is supposed to grow: a new network built on Architecture 2.0.

And now — the first tenants have moved into the new clusters.

**The first** is the [new, redesigned data storage system]({{< ref "blog/2026/04/18/схемы-данных" >}}). The new architecture set new requirements, and every core structure was rethought from the ground up. Technically, I could have migrated the data earlier — manually. But laziness is a great force that steers you toward solutions with the least effort. So I waited for the second tenant.

**The second** moved into the k3s cluster and settled in properly — with CI/CD and all the expected conveniences. This is the **database API**: a simple application that replaced a collection of bash scripts. It handles its tasks through **FastAPI**, lives in a container under k3s supervision, and has a distributed entry point across all nodes of the control cluster. Its capabilities are modest for now — load data into the database and export a snapshot to git. A humble skill set. But it was precisely these humble skills that made the move possible: open the door, and immediately drag the suitcases in.

In the future, I plan to use the API as the core for all future interfaces: a web panel, a personal dashboard, bot management, and CLI.

I'd love to say everything came up smoothly. But no.

Getting all the components to talk to each other — with certificates, tokens, and mutual authorization checks — turned out to be a non-trivial affair that, in the moment, generated a steady stream of colorful language. But in the end, everyone was successfully paired up, and they lived — happily ever after.

Well — I hope...
