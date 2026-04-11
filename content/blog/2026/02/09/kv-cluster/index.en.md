---
title: "KV Stores"
slug: "kv-stores"
date: 2026-02-09T12:00:00+00:00
draft: false
summary: "What KV stores are, why they matter for distributed systems, and how etcd became the foundation of operational data storage in Sigil Gate."
tags: ["architecture", "data", "infrastructure"]
categories: ["Architecture and Research"]
rubrics: ["Under the Hood", "Crossroads"]
---

Following up on the [previous post]({{< ref "blog/2026/02/07/data-storage" >}}), I want to take a closer look at KV stores and their role in the **Sigil Gate** network architecture. After all, this is a fairly niche topic — even within IT.

Last time I mentioned a KV cluster based on etcd — and realized that for most people this sounds roughly like a Latin incantation. I'll try to explain this technology in an accessible way without diving too deep into the technical weeds: what it actually is, where it's used, and why we chose etcd.

Buckle up — we're taking off!

## The General Concept

KV (key-value) is one of the oldest concepts in Computer Science. At its core, it's a data structure that stores pairs: a key and its corresponding value. Examples of key-value pairs are all around us:

- Passport number → owner's name
- Product SKU → its description
- Domain name → IP address
- Flight number → departure time

Structures like these are used when you need to quickly find something by a known identifier. Without going deep into theory, let's note the main advantage: such a structure provides nearly instant lookup of a value by its key, without scanning through the entire dataset. It works like a phone book — knowing someone's last name, you immediately open the right letter instead of flipping through the whole directory from cover to cover.

The concept is built on hash tables, first described by Hans Peter Luhn at IBM in 1953 — earlier than programming languages like Fortran (1957), COBOL (1959), and C (1972). The key point here is that this idea is not only far from new, but is actually much older than most modern programming languages.

In programming languages, this concept is implemented as collections commonly called "maps." They may be called dictionaries (Python), maps (Go, JS), associative arrays (PHP), or hash maps (Java) and hash tables (hinting at the hashing technology under the hood), depending on the context and domain. Implementation details vary, but at the level of the general idea — it's all the same thing.

But having a KV collection in the memory of a single program on a single computer is one thing. What if you need to store such data across multiple servers simultaneously — and guarantee that all of them see the same thing?

## From Dictionary to Infrastructure

In network infrastructure, plenty of tasks are essentially about working with key-value pairs:

- **Service configurations** — key: parameter name, value: setting
- **Service discovery** — "where does service X currently live?" (key: service name, value: address and port)
- **DNS** — essentially a classic KV store: domain → IP address
- **User sessions** — key: session token, value: user data
- **Distributed locks and leader election** — key: resource, value: who holds it
- **Caching** — key: query, value: result

The demand for solving such tasks naturally generated supply: the speed and simplicity of hash tables couldn't escape the attention of infrastructure developers. In this niche, the concept is implemented as KV stores, represented by a wide range of solutions from different developers, each focused on one aspect or another:

**Redis** — probably the most well-known. Stores data in memory, incredibly fast. It's used by Twitter, GitHub, StackOverflow — but specifically as a cache and message broker, not as a source of truth. Consistency isn't its strong suit: if a node goes down, data can be lost. For a cache, that's fine. For storing infrastructure state — it's not.

**ZooKeeper** — a veteran. Created at Apache for the Hadoop ecosystem, used by Kafka. Reliable, battle-tested. But: written in Java (heavy), complex API, non-trivial operations. ZooKeeper has a semi-humorous reputation in the industry: it's so complex to maintain that you need a separate zoo of engineers just to keep it running.

**Consul** (HashiCorp) — more than a KV store. It's an entire platform: service discovery, health checking, service mesh. It does have KV, but it's part of a larger ecosystem. A powerful tool, but for the task of "just storing keys" — overkill. Used when you need the full HashiCorp ecosystem (Vault, Nomad, Terraform).

**etcd** — created by the CoreOS team specifically for distributed configuration storage. Chosen as the foundation of Kubernetes — and that's probably the best endorsement possible. Compact, written in Go (single binary), simple HTTP/gRPC API, strict consistency through the Raft protocol.

All these solutions are optimized for speed and minimal latency, actively using RAM for fast data access (while etcd and ZooKeeper reliably persist data to disk). But they differ not so much in speed as in their approach to a far more fundamental problem.

## A Problem With No Perfect Solution

Beyond the task of real-time storage and processing, there's another class of problems that all the systems mentioned above are designed to address. These are the problems of data synchronization and information consistency.

Imagine you have hundreds of servers running — all serving the same service, working with the same data, and essentially performing the same function. Updates don't happen instantly or simultaneously across all nodes: when information is updated in one segment of the network, another segment might not know about it yet.

In such systems, there's always the challenge of updating stale information, resolving conflicts, and achieving three goals:

- **Consistency** — all nodes see the same data at the same point in time
- **Availability** — every request gets a response, even if some nodes are unreachable
- **Partition tolerance** — the system continues operating even if communication between nodes is disrupted

The problem is that these three goals are internally contradictory and mutually exclusive. This isn't a hypothesis but a mathematically proven limitation — the **CAP theorem**, formulated by Eric Brewer in 2000 and formally proven by Seth Gilbert and Nancy Lynch from MIT in 2002. A law of physics for distributed systems, if you will: **out of three — you can only choose two**.

Every decision in such a system is always a trade-off between response speed and data freshness guarantees, between availability and consistency, between simplicity and reliability.

And this is where it becomes clear why the KV stores listed above are so different — they take different approaches to finding a compromise in the same unsolvable problem:

- **Redis** chooses speed and availability, sacrificing strict consistency
- **etcd and ZooKeeper** choose consistency and partition tolerance, sacrificing availability when quorum is lost
- **Consul** — somewhere in between, with configurable consistency levels

## Raft: The Consensus Protocol

For the **Sigil Gate** project, we choose consistency and partition tolerance. There are different ways to address consistency, and one of the solutions is the **Raft protocol**.

Raft is a consensus protocol. It provides clear answers to how and in what sequence actions should be performed so that data in a distributed system remains consistent and is preserved even when individual nodes fail.

It works on a voting principle:

- One node is elected as the **leader**, the rest are followers
- All writes go through the leader — it distributes them to the others
- A write is considered successful when a **majority** (quorum) of nodes have confirmed receipt
- If the leader goes down — the remaining nodes elect a new one within milliseconds

Hence the odd-number rule: a cluster of an odd number of nodes **(3, 5, ...)** can tolerate the failure of up to **(N-1)/2** nodes while remaining operational.

This rule means that a cluster of 3 nodes survives the loss of 1, a cluster of 5 survives the loss of 2. A cluster of 2 nodes is useless — losing one means losing the majority. Like in a parliament: a decision is passed when the majority votes "yes," even if someone has left the room.

## Why etcd for Sigil Gate

**etcd** is a compact KV store built on the Raft protocol. It does one thing — and does it well. This is the "Unix Way" philosophy, and it fits perfectly with the tasks at the current stage — building a prototype requires straightforward solutions that provide basic functionality.

For our network, **etcd** covers the key requirements:

- **Strict consistency** — in a network where nodes make decisions based on data from the store (routes, access rights, statuses), you cannot allow a situation where two nodes see different states
- **Compactness** — a single Go binary, minimal dependencies. No need to bring in JVM (ZooKeeper) or an entire platform (Consul)
- **Simple API** — HTTP/gRPC, no magic. Easy to integrate with CLI and automation
- **Natural placement** — the etcd cluster is deployed directly on Core nodes, each node being a cluster member. No separate infrastructure is required at the initial stage.

This is a deliberate choice — because etcd is optimal for the current scale. Over time, we may transition to more comprehensive solutions — for example, Consul, which in addition to KV provides built-in health checking and monitoring system integration. But at this stage, etcd is exactly as much as we need. No more, no less.

