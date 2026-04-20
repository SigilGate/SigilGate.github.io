---
title: "New Network"
slug: "new-network"
date: 2026-04-16T12:00:00+00:00
draft: false
summary: "The concept behind Architecture 2.0."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["architecture", "infrastructure"]
---

We are building a new network.

All existing devices keep working for now — the transition period will last a couple of weeks while the new infrastructure is being set up. Once it's ready, all active connections will be migrated to the new servers.

The core idea behind the new architecture is distributed infrastructure. A cloud instead of a tree.

---

**First layer: entry nodes.** A cloud divided into cells. Each cell is a Kubernetes cluster tied to a single domain. This gives us flexible IP rotation through continuous node cycling: the topology never stands still, the address pool changes constantly. There are many cells, and domain names are not permanent either. The distribution mechanism routes each client along a different path every time — at sufficient scale, IPs and domains do repeat, but never beyond what looks like ordinary user traffic to familiar network resources.

**Second layer: exit nodes.** We'll containerize this part to open the door for new participants — without handing over server control or any administrative access. Onboarding is straightforward for anyone who can rent a server and run a container. Entry nodes route each participant to their own exit node — if an exit node is compromised, the participant simply repeats the process: connects a new server using the same scenario.

**Third layer: core infrastructure.** Data about connections, participants, and nodes — everything needed for routing control, backups, and operational management — goes into a separate distributed cluster.

---

The first two layers are the data plane, the working part of the network. The third is the control plane, the brain of the whole system. That's where we'll start building the new architecture.

First and foremost, this means working with data — including sensitive data. That calls for a higher security baseline. I've never been paranoid about this — but maybe it's time to start.
