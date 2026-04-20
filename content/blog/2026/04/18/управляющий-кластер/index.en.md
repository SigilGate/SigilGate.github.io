---
title: "The Management Cluster"
slug: "management-cluster"
date: 2026-04-17T12:00:00+00:00
draft: false
summary: "First component: the management cluster on Kubernetes."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["new", "kubernetes", "security", "architecture", "infrastructure"]
---

I had zero Kubernetes experience going in. Throughout my career it just never came up — the circumstances never aligned. Including it in the new architecture was a deliberate choice: I understand perfectly well how it works, and it fits exactly what we're building. It would have ended up in the project sooner or later. Well — time's up.

Nodes, pods, kubernetes, schmubernetes... It all turned out to be far less scary than it sounds. The tool is genuinely logical and well-structured. Yesterday I brought up my first cluster.

Two clusters, actually. I decided not to merge the Kubernetes control plane with the data store. Yes, it adds work: separate deployment, separate certificates, extra configuration overhead. But etcd lives as an independent distributed storage cluster — exactly as planned back in the old architecture. Down the road, this gives independent scaling for both layers. I'm currently debating whether to move the storage cluster closer to the entry nodes — to reduce latency for lookups at the network's edge.

Technically it all turned out straightforward, even if it took some time.

---

Time is the scarcest resource in this whole story. I work on this project in the rare gaps between everything else. I try not to let it go dark — but my main attention is elsewhere right now. I come back to it in the pauses. Right now, for instance: sitting in a café with a pastry and a coffee after physical therapy, typing this post on my phone — I always carry a keyboard for exactly these moments.

---

But the bulk of the time wasn't even the cluster itself — it was the adjacent security and operational questions. My goal isn't simply to stand up a working system. I'm also building the tools to automate all of this and reproduce it quickly and safely.

I try never to solve a problem in its specific, one-off form. I always try to reduce it to the general case — solve it once, reuse it everywhere. I wrote a substantial set of scripts covering the main administration scenarios: node and cluster deployment, certificate issuance, baseline security hardening. Debugging took time — but now spinning up a new cluster (or rebuilding an existing one) is a few minutes and a handful of terminal commands.

---

In parallel, I rethought the approach to security. Everything sensitive — keys, passwords, tokens — now lives locally. I'm also building a tool (working name: "Keyholder"): store everything sensitive on an encrypted physical device, administer nodes only through it, with explicit authentication at each session. The working pipeline is already assembled — and I may release it as open source once I've run it in a bit and cleaned up the interface.

A separate workstream — but one I enjoy, precisely because it means writing code and typing in the terminal again. I'd been missing that.

Yesterday: management cluster up. Tomorrow: entry node cluster. The day after: whatever comes next.
