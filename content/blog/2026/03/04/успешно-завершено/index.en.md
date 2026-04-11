---
title: "Successfully Completed"
slug: "successfully-completed"
date: 2026-03-04T00:00:00+00:00
draft: false
summary: "Yesterday I launched the subscription microservice and wrote 'deployed itself, without a single manual action.' This morning users couldn't connect. Not a single one."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["nginx", "incident", "rotation", "debugging"]
---

Moving forward is always moving into the unknown. You conquer a new summit, ship new functionality — and somewhere in that same motion, just off to the side, something quietly crumbles that was supposed to run without interruption. Not because you're careless. Every step just has a shadow, and it doesn't always fall where you're looking.

[Yesterday I was celebrating]({{< ref "blog/2026/03/03/осталось-реализовать" >}}): the subscription microservice was up, CI/CD had run, Nginx was configured. "Deployed itself, without a single manual action on the server." This morning users couldn't connect. Not a single one.

---

First hypothesis — Nginx. Reasonable: that's where things had changed the day before during microservice integration. I started diagnosing.

Both services were alive: Nginx and Xray on the Entry and Core nodes were running, no errors. Configs looked correct at a glance. The rotation log — two successful runs today. "Rotation completed successfully, routes updated: 1."

Successfully. Completed.

I stared at that line and felt that something, somehow, wasn't right.

---

Here's what actually happened.

The tunnel architecture works like this: a client connects to the Entry node via one gRPC path, and Entry forwards traffic to Core via a different — internal — path. **This is fundamental**: two different `serviceName` values on two segments, each with its own role. The client path is fixed and public. The internal one rotates automatically every hour plus/minus 30 minutes, roughly, to defend against statistical traffic analysis.

Nginx on the Core node must know the current internal `serviceName` — that's what it uses to route gRPC from Entry to Xray. The rotation script does exactly this: it takes the current value from the Xray config, finds it in the Nginx files, and replaces it with the new one.

The key word: **finds**. If Nginx has something different written in it — the script will silently pass by. Validation will pass (the config is syntactically valid), services will reload, the log will say "success." And Nginx will still have the wrong path.

That's exactly what happened. When the microservice was deployed, the Nginx config on Core was rewritten — and the **client** `serviceName` accidentally ended up in the gRPC routing block instead of the internal one. Easy to mix them up: both look the same, `api.v2.rpc.<sixteen characters>`. One is indistinguishable from the other on the outside. Where were my eyes?!

From that point on, every rotation dutifully updated Xray and Entry, but left Nginx on Core untouched — the string it was looking for simply wasn't there. The script had no way of knowing Nginx was broken. It honestly did everything it was supposed to and reported success.

The Entry node was sending traffic to Core with the current internal path. Nginx on Core found no matching `location` — and returned the cover site. Plain HTML. The gRPC connection fell apart.

---

The fix took a minute. Replace the wrong `serviceName` in Nginx with the correct one, reload. The next rotation ran properly — found the right string, updated it.

The incident lasted less than a day: the network broke on the evening of March 3rd, restored on the morning of the 4th. The cause — copy-pasting the wrong identifier during a manual config edit. Classic.

---

But what stays with me is something else. The rotation script ran "successfully" for several hours straight while the network was dead. Not because it was poorly written — the logic is clean, with rollbacks and validation. It just doesn't check the **consistency** between what's in Xray and what's in Nginx. It trusts that Nginx is already configured correctly — and only lays changes on top. If the foundation is crooked, it carefully places bricks on a crooked foundation and reports: "foundation accepted."

That's not a bug in the script. That's the boundary of its responsibility — the one you don't think about until it makes itself known.

And here I come back to where I started. Every step forward is not just a new opportunity, but a new layer of complexity laid on top of the previous one. The subscription microservice is a good thing, a needed one. But its arrival required changes to Nginx, and those changes required attention to detail that slipped at some point. The system became slightly more complex — and it immediately showed up where everything seemed long settled.

I'm not making a tragedy out of this. That's the nature of any living infrastructure: it's not static, it grows — which means it sometimes stumbles. What matters is not that the incident happened. What matters is that it could have been prevented. Add a consistency check on configs at the start of rotation. Run a smoke test after every pass — verify that the gRPC path is actually being proxied, not just syntactically present in a file.

Most likely, that's what I'll do. But first — this post.
