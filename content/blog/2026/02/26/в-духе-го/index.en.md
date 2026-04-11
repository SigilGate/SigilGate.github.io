---
title: "In the Go Spirit"
slug: "in-the-go-spirit"
date: 2026-02-26T00:00:00+00:00
draft: false
summary: "A microservice task — and a long-overdue reason to finally try Go in practice. Especially since our project is already soaked in it."
categories: ["Project Diary"]
rubrics: ["Margin Notes"]
tags: ["go", "microservices", "subscription"]
---

So, the next task on the list is a subscription microservice for the client segment. [I've already described the idea]({{< ref "blog/2026/02/22/ротация-путей-клиентский-участок" >}}): instead of a static VLESS link, the client gets an endpoint address where a small service lives. The client calls it — the service generates a current configuration and returns it. The connection path rotates, the user notices nothing, reconfigures nothing. Rotation happens invisibly — as it should.

Technically, it's not a complicated task. For things like this, I'd normally reach for **FastAPI** in Python — and most likely the first working version will be written in that. Familiar, no surprises. But while I was thinking through the task, another thought started taking shape.

Microservices? Seems like the right time to try **Go**.

I've made a few attempts before. The first encounter happened on the last working day of 2024. Officially we hadn't been let off yet, but everyone was already at the starting line. Somewhere online a headline caught my eye: "Go — a language you can learn in 15 minutes!" — and I took the bait. While colleagues were mentally assembling their New Year's salads, I was reading an introductory course. The language really did seem simple and interesting.

A few months later I finished a more thorough online course — and Go opened up in a completely different light. Everything turned out to be considerably harder than that first surface-level impression.

And here's the interesting thing: Go's complexity is a special kind. The language's vocabulary is small — but behind that brevity hide some fairly serious concepts. Concurrency and multithreading are baked into the language design, out of the box. There are pointer operations — not as low-level as C (no pointer arithmetic, for instance), but enough to understand why so many people with systems programming backgrounds are drawn to it.

And the main killer feature: **Go can pack all dependencies into a single binary**. No runtime environment, no interpreter, no virtual environments. Build it — and run it anywhere. Combined with fairly high execution speed, this makes it practically the ideal language for microservices.

All in all, Go is stylish, modern, concise — and full of other nice things — but somehow I'd never actually tried it in practice. The right moment just never came.

It seems the moment found me.

I looked at our project and suddenly noticed: **Go is already here**:

- **Hugo**, which builds this blog — Go,
- **Congo**, our theme — Go,
- **etcd**, the future data store in our infrastructure — Go.

The project had quietly, unbeknownst to me, become soaked in the spirit of this language. It would be strange not to write the subscription microservice in Go too.

Well then. That's the next step. Let's see what comes of it.
