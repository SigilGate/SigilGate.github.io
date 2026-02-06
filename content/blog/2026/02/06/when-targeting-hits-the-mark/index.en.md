---
title: "When Targeting Hits the Mark"
slug: "when-targeting-hits-the-mark"
date: 2026-02-06T12:00:00+00:00
draft: false
summary: "Searched for a singer named Annette — found the ANet project. Funny how targeted ads work when you're building a private network."
tags: ["reflections", "masking"]
---

It's funny how Google's targeted marketing works.

Yesterday at work we played a track by the singer Annette, and I decided to look her up: who she is, where she's from, that sort of thing.

That very evening, Google — apparently having adapted to my interests — served me a link about Anet. But a completely different Anet: [ANet](https://github.com/ZeroTworu/anet), a Rust-based VPN project with its own transport protocol.

I was genuinely surprised.

I felt like the protagonist of an old Soviet joke — about an emigrant arriving in Israel, greeted at the airport by a sign that reads:

> Remember! You're not the only Jew here!

Turns out I'm really not the only one.

The project, just like **Sigil Gate**, focuses on building private networks for small groups: friends, family, or close acquaintances. The repository greets you with an anime catgirl avatar and the following introduction:

> **ANet: A Network of Friends**
>
> ANet is a tool for creating a private, secure communication space between people who are close to each other. We build digital bridges where ordinary paths are unavailable.
>
> This is not a service. It's a technology for connecting those who trust each other.

In short, these folks also have plenty to talk about: connections, links, and how nothing is ever simple. They write mostly in Rust and dig pretty deep: their own client, their own server, and even their own transport protocol — [ASTP](https://github.com/ZeroTworu/anet) — which seems conceptually similar to QUIC.

## Some criticism

In my view, the main challenge today isn't encrypting traffic, and it isn't even wrapping it in a protocol that looks like white noise from the outside. That's exactly what the ANet team does — sending encrypted and obfuscated packets over UDP in random bursts.

The critically important challenge today is masquerading as regular TLS traffic.

{{< alert >}}
Any protocol — no matter how complex and opaque to an observer — that differs from normal traffic will inevitably be identified and either throttled to a crawl or blocked outright.
{{< /alert >}}

A solution like this might survive for a while — simply because it's too niche or too small-scale to attract attention. But sooner or later, they'll come for these solutions too. So I probably wouldn't adopt it myself.

But it's still interesting to look at.
