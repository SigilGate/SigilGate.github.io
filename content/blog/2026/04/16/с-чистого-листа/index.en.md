---
title: "From a Clean Slate"
slug: "clean-slate"
date: 2026-04-16T12:00:00+00:00
draft: false
summary: "Architecture 2.0 is taking shape — time to build. First component: a Kubernetes management cluster. But before any logic or services, there's one question that comes first: security."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["new", "kubernetes", "security", "architecture", "infrastructure"]
---

[Architecture 2.0]({{< ref "blog/2026/04/12/архитектура-2-0" >}}) is almost defined. The general shape is becoming clearer. Much is still rough — details will be worked out along the way — but the fundamental decisions are made. Time to build.

Exactly how to migrate from what exists to what's planned — I don't have a precise roadmap yet. Most likely I'll bring up the new infrastructure alongside the old and move things over gradually, piece by piece, until the old setup quietly fades away.

I'll start with the **management cluster** — a group of Kubernetes-managed servers handling service functions: PKI, etcd, the subscription service, Telegram bot, backups. The logic is simple: this component doesn't touch traffic — it can be brought up and validated in full isolation, without disturbing the working network.

The management cluster is the **brain of the entire system**. Losing it means losing everything. A single server won't do here: one point of failure at the most critical node isn't an architectural decision, it's an architectural mistake. We need a multi-server cluster — and if it's a cluster, it's Kubernetes. I've never spun up Kubernetes before. A good reason to finally dig in — I have a couple of free weeks, which should be just enough.

But before standing up any logic or building any components, there's one question that has to come first. **Security.** The management cluster is the foundation of all infrastructure, and this deserves serious attention. I can clearly feel the demand for security from the network's participants — that raises the bar. We're talking about operational resilience and about protecting sensitive data that will live on this cluster.

---

There are two approaches to security in the world.

**The paranoid one.** Build a reinforced concrete wall of layered defenses: castle walls, a moat, several fortifications along the approach. A password, then a password on the password, then a second factor on top of that.

**The easy one.** Don't overthink it. Accept that any system can be broken with enough determination and resources. Simply don't keep anything truly critical in the network. No sensitive data — no problem.

These approaches have an analogy in programming. One camp does everything to prevent errors: exception handling at every layer, propagation up the stack, crashing is never acceptable. The other follows the zen: it crashed — so what. Crashes are inevitable. Just bring it back up.

This second principle — **let it crash** — is what we're building into the network's architecture: any segment, any individual component can go down periodically, and the network keeps running.

I'll admit it: I've always been in the second camp. But this time I've decided to side with the paranoids. After all, we're talking about people.

---

So I decided to start over. From scratch. From a clean slate — completely rethinking the security approach at every level.

First step: organizational. The project moves to a separate account and is fully separated from everything personal. No mixing: **Sigil Gate** is now an independent resource with its own identity — a separate project with separate infrastructure.

Next: separate the layers. The first thing that migrates cleanly and quickly is everything related to communication: the blog, the channel. Clean up along the way: personal notes, noise, everything that accumulated since the pet-project days — goes to the personal account. Only content directly related to building the network stays in the blog.

---

All secrets move to a physical storage device (and are protected at the physical level). Format it under LUKS2 — the encrypted volume opens with a master password on each use.
Generate Ed25519 SSH keys with a passphrase. Now, even with access to the decrypted device, the key is useless without its password. Disable the SSH agent — the passphrase is requested on every connection, the key is never cached. Register the cluster nodes — verifying the host fingerprint from the hosting provider's panel on first connection. After verification, the host is locked in `known_hosts`, `StrictHostKeyChecking=yes`. Disable password login. Disable root access. Allow connections only from a defined user list. Update the system, configure updates for security patches only. Enable firewall, close all ports — keep only the service SSH open for now. Fail2ban blocks brute force. Write service user credentials and access tokens to the flash drive — together with a simple script that loads them into environment variables for the duration of a session. Forwarding environment variables with credentials is explicitly forbidden on the server side; all further actions are only from the operator's device.

Infrastructure is configured, servers are ready. Time to deploy the cluster.
