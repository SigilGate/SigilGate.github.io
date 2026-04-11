---
title: "The Challenge of the Age"
slug: "the-challenge-of-the-age"
date: 2026-02-28T00:00:00+00:00
draft: false
summary: "A prophetic article from Habr I couldn't find again — and the arms race it predicted. From OpenVPN to statistical invisibility."
categories: ["Architecture and Research"]
rubrics: ["Protective Runes", "Digital Borders"]
tags: ["DPI", "protocols", "VLESS", "regulation", "architecture"]
---

Somewhere around 2018 or 2019, an article appeared on Habr that I was never able to find again. Apparently deleted in one of the waves of purges under the law banning promotion of circumvention tools. A shame.

It was one of the first articles from which my acquaintance with the subject began. A good survey of solutions, arguments for and against running your own VPN — valuable in itself. But what was truly remarkable was something else: the author tried to look ahead. They took the trends of the day — before the sovereign Runet law, before the mass rollout of DPI, before the great wave of blockings — and tried to extrapolate. It's a thankless job, predicting. But most of the forecasts in that article came true.

If you manage to find that article — you'll be surprised how prophetic it turns out to be. I couldn't.

{{< alert "circle-info" >}}
**Upd:** I did find the article after all. As always: in my memory everything had stayed completely different... Still — many of the topics in that piece are still interesting and relevant today, and it reads as if written right now. For the curious: [Tunnels and VPN Resistant to DPI](https://habr.com/ru/articles/415977/).

As it turned out, it wasn't deleted from Habr — but the author is no longer the author.
{{< /alert >}}

## The Toolkit of the Era

In those years I was only beginning to build my first VPN networks. The choice of tools was straightforward.

**OpenVPN** — the main "people's" VPN of the era. Reliable, flexible, well-documented. But heavy — and recognizable: its characteristic TLS handshake and traffic patterns made it visible to any DPI that wanted to see it. **SSH forwarding** — the other extreme: a minimalist tunnel over SSH, mimicking service traffic. Fine for targeted tasks, not for everyday use. **Shadowsocks** emerged as a direct response to the Great Firewall of China: a proxy masquerading as an encrypted stream with no obvious protocol markers, built specifically to counter DPI — and for a long time it succeeded. **WireGuard** stood apart: simple, fast, elegant — but transparent. No obfuscation, traffic is identified instantly. Speed in exchange for stealth.

I eventually settled on **IKEv2** — a corporate standard unpopular with enthusiasts. That's exactly why it stayed below the radar for so long: no mass adoption — no stable patterns — no priority for blocking.

But the most interesting thing in that article was not the taxonomy of solutions. The author pointed to a trend that was only just beginning to take shape: encryption alone was ceasing to be enough. The next frontier — **obfuscation**: traffic had to become not just encrypted, but unclassifiable, indistinguishable from any known protocol. And beyond that horizon, the author predicted the emergence of protocols with **mimicry**: a VPN pretending to be an ordinary web server. You knock — a page of cats. You knock correctly — a tunnel.

A few years later, we saw exactly that.

## The Prediction Came True

The pioneers were **VMESS**, followed by **VLESS**. The shared principle across them and the analogues that came after: TLS encryption, minimal header overhead, mimicry of a live HTTPS host, a friend-or-foe system at the handshake level. Fallback to a real web server — for everyone who knocked the wrong way. No TLS over TLS or other heavy schemes that themselves become a demask marker. Implementation details vary across protocols — handshake order, data encryption level — but the principles are constant.

The protocols proved so effective that major commercial VPN services began one by one releasing their solutions as open source — building trust and inviting audit.

It seemed: the problem was solved.

## The Arms Race

But for every tricky nut there's a bolt with matching threads. And for the bolt — a situation with no easy way out. And so it goes, spiraling, without end.

A familiar story — the sword-and-shield arms race. Detection systems don't stand still: smarter traffic analyzers appear, active probing, heuristics based not on packet contents but on the behavior of the connection as a whole.

Where we stand now: the best modern protocols have still not been cracked. But the search is in an active phase. And the primary direction of attack chosen by regulators is not breaking cryptography. It's identifying **traffic behavior patterns**: characteristic signatures by which a node can be detected and blocked precisely. Sometimes "precisely" means entire subnets or taking down major hosting providers. But the principle is the same: don't break the lock — learn to recognize the right door.

From this — **the main challenge is not finding the right protocol.** Protocols will keep appearing, each better than the last. The challenge is different: making a connection **statistically invisible** — indistinguishable from background traffic by volume, rhythm, or behavior.

From this vantage point, the "rent a VDS abroad and tunnel through it" scheme is dead. Such channels will be detected and blocked. And as DPI develops, the time it takes will only shrink.

Wrapping traffic in a tunnel is no longer enough. It has to be dissolved into the background. How exactly — that's for the next part. And (though it's a thankless job) I'll try not just to describe the principles, but to look a little further ahead. Who knows — maybe in five or six years someone will cite me too?
