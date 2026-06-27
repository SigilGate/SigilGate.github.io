---
title: "Core Services for the New Architecture"
slug: "core-services-for-the-new-architecture"
date: 2026-04-24T00:00:00+00:00
draft: false
summary: "The control cluster is deployed, data migrated, API serving over HTTPS. A long list of things still ahead — here's how to pick only what's strictly required to get the network running."
categories: ["Дневник проекта"]
rubrics: ["Хроники проекта"]
tags: ["api", "архитектура-2.0", "этап-3"]
---

The control cluster is deployed. Data migrated. The API is up and responding over HTTPS. Seems like everything is ready.

And then a list appears in my head.

A long list. Architecture 2.0 isn't just "API instead of scripts." It's a full operating system for the network: complete CRUD for users and devices, node rotation, an appeals mechanism, an operator CLI, a new Telegram bot, periodic state snapshots, migrating Core nodes to a phone-home scheme... Quite a lot, actually.

You want everything at once. But reality is this: to get the network running in even a minimal form, you don't need "everything at once" — you need exactly the minimum without which you physically cannot connect a single client.

I formulated that minimum as seven tasks, and spent the last couple of days on them.

**NodeService** — phone-home for Core nodes. A node comes up, sends a request with its IP, domain, and service names, and registers itself in etcd. Without this, the cluster has no knowledge of the node's existence — it's physically there, but for the network it doesn't exist.

**TokenService** — join token mechanism. The operator generates a one-time token with a 24-hour TTL. When registering, the node presents the token and gains write access. Without this, anyone could register anything with a single POST request.

**CellService** — a logical abstraction over nodes. A cell is a Core node with an assigned domain: a separate traffic exit point. CellService reads nodes from etcd and returns the domain plus service names. It writes nothing itself — read-only.

**RouteService** — device routing. Each user device is bound to a specific Core node. RouteService knows this binding: fetch a route, update it, list them. Without routes, the subscription doesn't know where to send a client's traffic.

**UserService** and **DeviceService** — both read-only for now. The first checks that a user exists and is active. The second lists their devices and returns the UUID of each — the client identifier in the VLESS protocol. Full management comes later. For now, read access is all that's needed.

And finally — **`GET /subscription/{user_id}`**. A public endpoint, no authorization required: the user doesn't need to know any tokens, only their own ID. The service assembles the chain: checks the user → fetches devices → finds a route for each → looks up the cell's domain → constructs the VLESS link. Returns plain text, one link per line. This is the subscription URL — the one the user pastes into their client.

Result: seven services written. 151 tests green. Push to the cluster.

But the very first rough check under near-production conditions reveals three bugs.

**First** — in TokenService, in the revoke operation. I expected revoke to delete the token. It didn't — it wrote a `used=true` flag, leaving the record in etcd intact. A revoked token continued to exist in storage, just marked as used. Fix: delete all keys in a transaction. The token should disappear, not be flagged.

**Second** — in NodeService, during registration. On a repeated POST from an already known IP, the node silently overwrote the domain and service names. The status was preserved, everything else was not. One extra request and a node is running with someone else's configuration, without a single warning. Now a repeated registration returns 409. Want to change a node — there's PATCH for that.

**Third** — in ApiTokenService. Every API request goes through Bearer token validation, but the `last_used_at` timestamp wasn't being updated. A token could be used hundreds of times in a row, and in etcd — silence, as if no one had ever touched it. Now, on every successful authentication, the service writes the timestamp to storage.

Fixed, tested, deployed. Everything seems to work as planned.

Next stage: the entry cluster. Existing entry node servers are connected as Kubernetes worker nodes, Entry pods are deployed on them. The first step toward routing user traffic through the new scheme — from client to internet.
