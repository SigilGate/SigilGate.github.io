---
title: "Architecture 2.0"
slug: "architecture-2-0"
date: 2026-04-12T12:00:00+00:00
draft: false
summary: "Breaking down what was wrong with the old architecture and how I redesigned it: from a tree to two independent clusters, zero trust, one domain per cluster, and Docker-based onboarding."
categories: ["Architecture & Research"]
rubrics: ["Crossroads"]
tags: ["architecture", "security", "kubernetes", "zero-trust"]
---

[The cryptominer attack]({{< ref "blog/2026/04/12/атака-криптомайнера" >}}) last week was a trigger — not the cause. By the time of the incident, a sufficient list of architectural limitations had already accumulated, growing more obvious as the network scaled. I'd understood for a while that incremental patches wouldn't cut it here.

The decision: rethink everything. Not individual components — the principles the architecture is built on.

---

The core limitations:

**Exit node shortage.** Buying servers abroad is getting harder. I'd started bringing in participants who run Core nodes themselves — but the connection mechanism remained manual: SSH access, manual setup, manual oversight.

**Domain name shortage.** The old architecture required a separate domain for each node — entry and exit. At scale, this became an operational constraint: not enough domains to onboard new participants even when server resources were available.

**Trust between core nodes.** Core nodes were connected in a trusted SSH mesh. Convenient for management — dangerous in operation. Compromising one node opened access to all the others. That's exactly what the incident exploited.

**Rigid tree topology.** Each core node anchored its own group of entry nodes. At scale, each such node became a potential bottleneck. The layers couldn't scale independently.

---

What changed.

**From a tree — to two independent clusters.**

The entry layer is a uniform Kubernetes cluster. Any Entry pod can serve any client. A new participant adds a worker node — pods are distributed automatically. The exit layer is federated: independent Core nodes run by participants. The layers scale independently; the number of entry and exit nodes is no longer coupled.

Solves: rigid topology, scaling problems.

**Zero trust between nodes.**

Nodes don't trust each other by default. All communication goes through mutual certificate authentication (mTLS). The management cluster is the sole certificate authority. No SSH trust, no trusted networks between core nodes.

Solves: lateral movement when a single node is compromised.

**One domain per cluster.**

The entry layer lives under a single cell domain. The exit layer does too. An Entry pod connecting to a Core node sends the cell's SNI — regardless of the actual IP of the specific server. To an observer, all traffic from the cell looks like interaction with a single service.

Solves: domain name shortage; one domain now serves an arbitrary number of nodes.

**Subscription instead of static config.**

The client holds a subscription URL, not a specific address. On update — the app fetches the current configuration automatically. Node changes, gRPC path rotation, cell switching — all of this happens transparently for the user.

Solves: operational overhead when rotating nodes and configurations.

**Docker-based onboarding.**

A participant installs Docker, receives a one-time join token, runs the container. The container generates a key pair locally, sends a CSR to the management cluster, receives a certificate, and registers in the network. The private key never leaves the participant's server. No SSH access is handed over.

Solves: operational complexity of onboarding; adding new participants means a join token and running a container.

**Dedicated management cluster.**

A separate, independent cluster of management servers running Kubernetes control plane, HA etcd, and a Root CA — fully separate from the data plane. Service infrastructure lives here too: Telegram bot, backups, subscription service. The management cluster doesn't carry traffic — only management and PKI.

Solves: single point of failure; resilience through a distributed server cluster.

---

The result: the network scales horizontally in both layers — quickly and without manual operations. A compromised node or even an entire cell is an isolated incident, leaving everything else untouched. Participants rebuild their infrastructure from scratch through the same onboarding mechanism.

Detailed technical documentation for Architecture 2.0 is available in the docs section (Russian only).
