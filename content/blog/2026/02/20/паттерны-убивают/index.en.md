---
title: "Patterns Kill"
slug: "patterns-kill"
date: 2026-02-20T00:00:00+00:00
draft: false
summary: "The story of how I lost my VPN in the November wave of blockings — and what it taught me about the nature of DPI: behavioral patterns are more dangerous than any vulnerability in the protocol itself."
categories: ["Editorial Perspective"]
rubrics: ["Starting Point"]
tags: ["VLESS", "DPI", "regulation", "reflections"]
---

Last autumn I lost my VPN. I mentioned it briefly [in one of my earlier posts]({{< ref "blog/2026/02/19/особенный" >}}). The classic "samovar" setup: a VDS at a foreign hosting provider, set up by hand, no middlemen. Reliable, dirt cheap — and I got used to things just working. The autumn wave of blockings swept it away along with thousands like it. It wasn't just solo operators like me: large commercial services went down too — some partially, some entirely.

The incident made me stop and think. Not about how to restore access quickly — that's solvable. But about the nature of what happened: why did a scheme that had worked for years suddenly break?

Honestly — I had grown too complacent. I stopped paying attention. The warning signs were there: back in May, the first reports about problems with **VLESS** — the protocol underpinning most "samovars" — started appearing in the press.

The November collapse was a shock. A protocol that had seemed unbreakable had fallen. The media framed it as a great achievement of the Russian regulator: Russia had accomplished what China could not — a country with the most sophisticated internet censorship system in the world, with decades of experience and enormous resources. The Great Firewall of China, for all its power, had found no answer to **VLESS**. The Russian regulator had.

When I began piecing together what had actually happened, though, the picture turned out to be less grim. **The protocol held.** Nobody has cracked it to this day. So what happened? They were all brought down by **shared patterns**: the characteristic traffic signatures that DPI systems had learned to detect and isolate from the general flow. The protocol itself was blameless. The problem was the people using it — and the way they used it.

My first encounter with **VLESS** came in the early months of its existence — a coincidence of timing. When I set up my first Xray server, the protocol had just been released and working clients could be counted on one hand. No web panels, no convenience layers: configs were written by hand in the terminal, directly on the server. It ran that way for years — without much attention from me, needing a restart only occasionally.

In May of last year I finally decided to investigate why the server was dropping intermittently. The cause turned out to be mundane: several client devices running different operating systems were registered under a single account, and simultaneous connections were causing a conflict somewhere. A known problem, a trivial fix — create separate accounts for each device. But while I was at it, something made me check what had been happening in the VPN world over the past few years.

Here's what had happened. **VLESS** was still relevant and unbroken — nothing better had come along. But it had become enormously popular — and that, I now understand, was the first warning sign I missed. Popularity brought convenience and tooling: web panels, ready-made scripts, utilities for every taste. I went soft. I installed the slick 3X-UI panel without bothering to change the default port or mask it in any way. Xray, for its part, was running without any disguise — directly on port 443.

I had become one of those ten million lemmings who can't possibly be wrong. I gave in to the collective euphoria, put my faith in **VLESS**'s invincibility — and threw all caution aside. I started doing what everyone else was doing. That's what did me in.

That's exactly what the DPI systems were detecting. Millions of users following the same playbook: identical setup guides, the same traffic mimicry targets — microsoft.com, cloudflare.com, apple.com — the same tools and panels with default ports that turned out to be well known not just among enthusiasts. On top of that came active probing from the regulator: according to some reports, DPI systems were sending specially crafted TLS packets with reordered bytes in the ClientHello — the first message a client sends when introducing itself to a server during connection setup. A real web server responds to such a request predictably. Xray without a properly configured fallback responds differently. That difference in behavior was a pattern too — and detection systems had learned to spot it.

The result of the regulator's campaign was not a compromise of the protocol, but the identification and compromise of the nodes running it — followed by IP-level blocking. I saw this firsthand: in a frantic attempt to restore the service, I lost SSH access to the node — but only from within the Russian network. From foreign servers, it remained reachable.

The **VLESS** blocking campaign was unprecedented in scale. It made one thing unmistakably clear: the era of placing absolute trust in a single technology — no matter how sophisticated — is over. No one can feel invulnerable anymore. Even the most advanced solutions are now at risk.

But the story has another side. The campaign also exposed the regulator's own weaknesses. When all resources were thrown at **VLESS**, many previously blocked protocols suddenly began working again. For a brief window, access to previously unreachable resources opened up — apparently there wasn't enough hardware capacity to block everything at once: total control had run into hardware limits. The situation varied dramatically depending on region, provider, and connection type, which added to the confusion and made any generalization nearly impossible.

Still, the main conclusion drawn by most players in the market was unambiguous: things will never be the same. The schemes that worked before — rent a server abroad for the price of a coffee, tunnel through it, get your own personal VPN — no longer work. Those channels will be identified and blocked. And as DPI systems and techniques continue to evolve, the time to detection and compromise will only shrink.

The era of simple schemes is over. Keeping a network running reliably in the future will require more complex infrastructure solutions — scalable and diversified, capable of adapting to regulator actions: shifting topology and protocols as needed, and achieving ever deeper levels of concealment.
