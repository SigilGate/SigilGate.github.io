---
title: "Path Rotation: The Client Side"
slug: "client-path-rotation"
date: 2026-02-22T18:00:00+00:00
draft: false
summary: "gRPC path rotation on the Entry→Core segment has been working for a while. Time to close the same gap on the client side — and we finally have a scheme."
categories: ["Architecture and Research"]
rubrics: ["Protective Runes"]
tags: ["безопасность", "маскировка", "xray", "grpc", "подписка"]
---

[Last time]({{< ref "blog/2026/02/22/как-развивать-проект-не-накапливая-технический-долг" >}}) I wrote about technical debt. That topic apparently hit hard enough that I decided to tackle one of the long-postponed tasks.

Early in the development of **Sigil Gate**, we worked on rotating gRPC paths on the Entry→Core segment — adding «statistical noise» so that constant traffic through the same endpoint wouldn't become a recognizable pattern. I [wrote about that]({{< ref "blog/2026/02/01/path-rotation" >}}) already. But the same problem on the segment from the client to the Entry node had remained unsolved.

Today I finally got to it.

Had a pretty intensive discussion with friends. Learned a lot — in particular, about the **subscription** mechanism supported by almost every VPN client.

The idea is simple. Instead of handing the user a static VLESS link, we give them a **subscription URL**:

```
https://<DOMAIN>/api/<UUID>
```

The client periodically fetches this URL and pulls the current configuration on its own. If the `serviceName` on the Entry node changes — the next request returns an updated link. The user does nothing, reconfigures nothing. They just hit the microservice running on the entry node.

The user's entry point is a domain tied to the Entry node. When the Entry is replaced, only the DNS A-record changes — the subscription URL stays the same.

The scheme is designed — conceptually, for now, and documented. All that's left is to build it.
