---
title: "gRPC path rotation"
slug: "ротация-grpc-путей"
date: 2026-02-01T18:30:00+00:00
draft: false
summary: "It's been on the roadmap for a while — and it's finally done. The most vulnerable segment of the network is now protected by automatic address rotation."
tags: ["new", "security", "obfuscation", "xray", "grpc"]
---

It's been on the roadmap for a while — and it's finally done.

In our architecture, the most critical segment is the connection between the entry node and the core node. This is the cross-border channel: it crosses the perimeter where DPI systems focus their attention.

VLESS over gRPC is a strong combination. The traffic is virtually indistinguishable from regular HTTPS: TLS with a valid certificate, HTTP/2, browser-like TLS fingerprint. From the outside, it looks like a request to an ordinary web service. Identifying it by signatures is practically impossible.

But "virtually indistinguishable" does not mean "impossible to detect."

Even when the traffic content is encrypted and the protocol is obfuscated, metadata remains: who, where, how often, how long. A persistent connection to the same gRPC path on the same host is a pattern. Even for a legitimate gRPC microservice, this looks suspicious — real services typically call different endpoints.

If the path doesn't change for weeks, it becomes a marker. Statistical analysis can single out such a connection from the general traffic flow.

The solution: rotate the gRPC serviceName automatically at a set interval. Each new path is a randomly generated string. From the outside, it looks like activity from various services and users on the same host.

I'd been putting off the implementation for a while, but today I finally got around to it.

In its minimal form, it's a bash script on the core node, triggered by a systemd timer. The script generates a new random path, updates the Nginx and Xray configs on the core node, then pushes the outbound path change to each entry node over SSH.

An important detail: the client-facing side is not affected. The path is rotated only on the internal entry→core segment. Clients don't need to reconfigure anything — they keep connecting to the entry node at the same address as before.

## Current limitations

For a few seconds during rotation, the connection may drop — services are restarted. The client reconnects automatically, but the interruption is noticeable.

In the future, we plan to switch to the Xray gRPC API (hot reload). This will allow path changes without restarting services — making the rotation seamless, with no connection drops.

Currently, rotation is set to run every hour. Practice will show whether this causes any noticeable inconvenience on the client side. If not, we'll increase the interval or move rotation to periods of least activity.
