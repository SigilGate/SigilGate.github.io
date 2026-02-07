---
title: "The Data Storage System"
slug: "the-data-storage-system"
date: 2026-02-07T12:00:00+00:00
draft: false
summary: "Wanted to create a couple of user accounts — ended up designing a data storage system. A typical weekend."
tags: ["architecture", "data", "documentation"]
---

This weekend I planned to dedicate to simple, straightforward things: create a couple of new user accounts for connecting new users and client devices. And maybe automate the process along the way — so I wouldn't have to do it all by hand next time.

Not much work. Just add a couple of accounts to the configs.

Or so I thought.

In reality, I'm still in the middle of pulling apart the monorepo where everything piled up during the prototyping phase. In that same heap lie user data — configs, credentials, connection parameters. The first idea was simple: move it all into a separate private repo and keep it there, updating manually from time to time. But the volume of information keeps growing, and what's been done by hand on the fly all this time — it's time to turn into a proper process.

And to properly automate typical tasks — creating users, configuring connections, generating client configs for entry nodes — the first thing you need is somewhere to store all that data. Systematically. In a way that lets you work with it and process it programmatically.

And at that point, I had a rather vague idea of how to organize this.

## Two contradictory requirements

Because two opposing requirements emerge at once.

On one hand — the data must be stored centrally. It must always be accessible, even in the most critical moments, in case of failure and loss of access to all network nodes. Even if disaster strikes — the data must be recoverable, and the network must rise like a Phoenix from the ashes.

On the other hand — the network is decentralized by nature and must be fault-tolerant. The data needed for real-time control and management must be distributed across nodes.

At this point my head would usually explode, and I'd tell myself:

> I'll think about it tomorrow.

## Tomorrow has come

But apparently that "tomorrow" has finally arrived. I sat down and seriously thought about how to elegantly organize data storage in our network.

After hours of deliberation and lengthy consultations with friends, a **hybrid architecture** of two layers was chosen:

- **KV cluster** based on `etcd` — a distributed operational store deployed on Core nodes. Fast access to the current network state, strong consistency via the Raft protocol, resilience to individual node failures. This is what the network works with in real time.
- **Git repository** — data versioning and backup. Stores the desired infrastructure state, provides versioning, change auditing, and — most importantly — the ability to fully restore the network from scratch. Even after losing the entire operational infrastructure.

Bidirectional synchronization ties both layers together: Git → KV during deployment and recovery, KV → Git for periodic persistence of operational changes.

This approach satisfies both requirements at once: decentralized operational management through the KV cluster, and guaranteed data preservation and recovery through Git.

For now it's a design — implementation lies ahead. But the general principles have been formulated and documented. Details are in the [Data Storage]({{< ref "docs/data_storage" >}}) section of the documentation.

---

P.S. I never did create those new accounts — because I only have one pair of hands (admittedly, rather clumsy ones) and one pair of eyes (notably, already quite red). But the weekend isn't over yet — so there's still hope.
