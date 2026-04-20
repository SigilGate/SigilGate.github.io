---
title: "Data Schemas"
slug: "data-schemas"
date: 2026-04-18T12:00:00+00:00
draft: false
summary: "The etcd cluster is up and waiting for data. Here's why copying everything over as-is didn't work out — and how the migration turned into a full rethink of the data schemas."
categories: ["Architecture & Research"]
rubrics: ["Crossroads"]
tags: ["etcd", "architecture", "data-schema", "architecture-2.0", "new"]
---

[The cluster is up.]({{< ref "blog/2026/04/16/с-чистого-листа" >}}) Three nodes, k3s, HA etcd — everything works and is waiting. Just empty. No data.

The first thought in a situation like this: take what you have, move it as-is, figure things out later — add fields, drop extras, adjust to the new requirements over time. Sounds reasonable — which is exactly why I didn't abandon the idea quickly.

---

In the old architecture, data lives as JSON files: one folder per entity type, one file per instance. A user is `users/1.json`. A device is `devices/<uuid>.json`. A route is `routes/entry_<entry-IP>_core_<core-IP>.json`. And so on.

When I designed that, I already had the future move to a KV store in mind — hence the choice of JSON as the format. So the first migration option was exactly that: a JSON blob as a single key.

```
/sigilgate/users/1  =  {"id": 1, "username": "<USERNAME>", "status": "active", ...}
```

Brought over the filesystem — put it in etcd. The only difference is that the key is now a path instead of a filename.

The second option: pull primary keys out of the JSON body into the path, and split each object into individual fields. The path becomes the identifier; the value holds only the data.

```
/sigilgate/users/1/username  =  "<USERNAME>"
/sigilgate/users/1/status    =  "active"
```

---

The first option has its own logic. The object is always in one place — read one key, get everything. The schema is simple, familiar, close to what already exists. Nothing needs to be reinvented.

But it has an uncomfortable property. Every field update is a read-modify-write: fetch the object, change the field in memory, write it back. In a system where gRPC paths rotate every hour and Entry pods look up the database on every client connection — that's not just overhead, it's a potential race condition. And the primary key, by the way, still gets duplicated: `"id": 1` in `1.json` never went away — it just moved from the filename into part of the path.

The second option eliminates these problems. Updating one field is a single atomic PUT, no read required. Partial reads fetch exactly what's needed, no full object retrieval. The primary key lives in the path and nowhere else. The tradeoff: reading a whole object becomes a range query, and creating an object requires a transaction to avoid leaving a half-assembled record in the database.

After reviewing all the entities, the choice was obvious. The most frequent operations in the system are single-field updates, single-key lookups, single-status checks. Reading whole objects is rare. They're small — range queries are not a problem here.

---

But "which option" turned out not to be the most interesting question. The more interesting one was: does everything that exists even need to come along?

Migration isn't just a format change. It's an opportunity to stop and ask: "Do we really need to drag all the old clutter into the new store?"

I did some cleanup before the move. Removed fields that had been added "for the future" that never arrived. Removed things that became obsolete when the schema changed. Removed annotations and service artifacts that carry no functional weight. Clutter accumulates in any living system — sometimes you need to shake it out and throw away what turned out to be unnecessary, so the path forward is lighter.

---

In parallel, I made a more significant decision: split the data into several independent layers.

**Administrative layer.** Everything related to servers as hardware: hostname, location, provider, technical specs, diagnostics, certificates. This is Keyholder's territory — the operator tool I've [written about before]({{< ref "blog/2026/04/12/архитектура-2-0" >}}). SigilGateApp doesn't know this data and shouldn't. Before the migration, this slice was simply dropped from the database.

**Communication layer.** Some users want to register, get support, stay informed — for that you need contacts, names, and ways to reach people. But this is de-anonymizing information, and keeping it next to operational data is wrong. Moreover, for many users, even membership in a public group is unacceptable. So everything related to communication — support conversations, newsletters, notifications — moved to a separate namespace, `/public/`. A separate layer that can eventually move to a separate cluster.

**Operational layer.** Everything needed for the network to function: nodes, routes, users, devices. Only anonymized data — no contacts, no explicit identifiers. Telegram IDs are no longer stored. There is no direct link between layers in the database.

---

The data changed substantially as a result.

Roles and domains are no longer separate entities — they became node attributes. Instead of `/sigilgate/roles/core/<ip>`, it's now `/sigilgate/nodes/<ip>/role = core`, and the domain is just a field on the same node. Devices moved under users: `/sigilgate/users/<id>/devices/<uuid>/`. Routes were completely redesigned: the old Entry-IP × Core-IP scheme lost its meaning — now it's device UUID → Core node, a direct lookup that the Entry pod performs on every client connection.

---

Once the schema was settled, the rest was mechanical. I wrote two scripts: one transforms and exports data from the old store into a JSON snapshot; the other loads the snapshot into etcd. The result: 342 keys, ready to load.

Along the way I also wrote a backup service. The scheme is what was planned from the start: etcd as the primary store, a Git repository as the backup. Periodic dumps from etcd to JSON, committed to the repository.

etcd has its own snapshot mechanism — and it will be running too. But the Git backup solves a different problem. An etcd snapshot is a binary image of the entire cluster: perfect for full recovery after a catastrophe, but useless if you need to understand what changed three days ago or roll back a single record. The Git backup stores data in human-readable text format — open the diff and see the exact change, restore a specific record without touching anything else. Two tools — two different recovery scenarios.

Loading and validation are the next step.
