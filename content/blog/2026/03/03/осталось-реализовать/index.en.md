---
title: "Done Implementing"
slug: "done-implementing"
date: 2026-03-03T21:00:00+00:00
draft: false
summary: "Ten days ago I wrote 'the scheme is ready, all that's left is to implement it.' Today the subscription microservice is live."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["microservice", "subscription", "docker", "automation", "fastapi"]
---

[Ten days ago]({{< ref "blog/2026/02/22/ротация-путей-клиентский-участок" >}}) I wrapped up a breakdown of the client configuration delivery scheme and closed with a line in my characteristic style: "the scheme is locked in documentation. All that's left is the small matter of implementing it." Phrases like that in a project diary are a dangerous sign. They sound optimistic, the future seems close — and a month later you look back at them with quiet reproach.

This time we got lucky. Today the service is live.

## The Subscription Mechanism

A quick reminder of the problem — [in that post]({{< ref "blog/2026/02/22/ротация-путей-клиентский-участок" >}}) I explained why a static VLESS link is an Achilles' heel. We had learned to rotate gRPC paths between Entry and Core nodes automatically, on a schedule. But the client knows nothing about this: they have a link copied at registration, and it goes stale sooner or later.

The solution — a **subscription mechanism**. Instead of a static link, the user gets a single address:

```
https://<domain>/api/<UUID>
```

The client app periodically calls it and fetches the current configuration on its own. If the `serviceName` changed — the next request returns an updated link. If we replaced the Entry node — we redirect DNS, the subscription URL stays the same. The user doesn't need to do anything — the system updates the configuration by itself, without any extra steps.

## Microservice Architecture

Now for what's inside.

The microservice is small — around 270 lines of Python. FastAPI, four layers with one-directional dependencies, functional style. No databases: data comes from a local clone of the data repository that already lives on the Core node and syncs every 15 minutes.

The logic of one request: UUID from the path → check the device → check the user → find active routes for this Core node → for each route, match an Entry node domain → build a VLESS link → pack into base64 and return. One response can contain multiple links — if the user has access through several Entry nodes. If the device is blocked, the user is inactive, or there are no routes — just a 404, no details.

The service is containerized in Docker, running on the Core node. Nginx proxies `/api/` to localhost; the service isn't directly accessible from outside. CI/CD follows the familiar scheme: push to `main` → GitHub Actions → SSH → `git pull` + `docker compose up --build`. Deployed itself, without a single manual action on the server.

## FastAPI

[A bit earlier I wrote]({{< ref "blog/2026/02/26/в-духе-го" >}}) about how this microservice was a good chance to finally try Go in practice. The language is already present throughout the project: Hugo, the theme, the future etcd — all Go. It would have been logical to fit the subscription service into that picture too.

But in that same post I was honest about one caveat: the first working version — in FastAPI. Familiar, no surprises, fast path to a result. And so it went. The prototype is written in Python, it works, it does the job.

Go isn't going anywhere — it's just the next step. We'll rewrite it someday.

## Results

What this changes in practice.

Before, any change — a node swap, a path update, a scheduled rotation — meant somehow getting a new link to every user. Now infrastructure can change independently: clients will get the updated configuration on their next sync, on their own.

"All that's left is the small matter of implementing it." Well, here we are. Implemented.
