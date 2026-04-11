---
title: "No Guarantees"
slug: "no-guarantees"
date: 2026-02-26T00:00:00+00:00
draft: false
summary: "People often ask about the security of our network and what guarantees we offer. Here's an honest answer — no marketing promises, no fine print."
categories: ["Architecture and Research"]
rubrics: ["Protective Runes"]
tags: ["security", "VLESS", "TLS", "VPN", "DPI"]
---

People often ask me: how secure is your network? What guarantees do you offer?

I want to answer honestly — and as simply as possible, without technical jargon. Fair warning: I probably won't manage that last part. I made my peace with that a long time ago.

## What You Have to Accept

Let me start with the uncomfortable part. At least it's honest.

**Any private network you connect to can see your traffic.** This isn't a bug, an oversight, or a flaw in any particular solution. It's a fundamental consequence of how networks work. When you send data through someone else's infrastructure, you automatically accept the risk that the administrator of that infrastructure has access to it. First in that chain is your internet provider. Next is whoever runs the network you just joined.

When two devices establish a connection over the internet, data packets travel through dozens of intermediate nodes. Nobody knows in advance which ones: routing rules in the global network are complex and unpredictable. This is what makes a **Man-in-the-Middle** attack possible — when someone positions themselves between sender and recipient and reads everything passing between them. The attack was first theorized in the 1980s by cryptographer Whitfield Diffie — the same person after whom the Diffie-Hellman algorithm is named, which underlies TLS today. The irony is that the person who described one of the greatest threats also created the primary tool to defend against it.

When you connect to a private network, you make your traffic path **predictable**: it flows through the network's nodes, and routing inside is governed by the administrator's policy. This gives the administrator full control over your traffic's movement — which means a Man-in-the-Middle attack is most likely to come from that direction.

To be direct: by joining our network, you accept the risk that I — or any other administrator of our infrastructure — am technically capable of intercepting your traffic. This is an unavoidable reality you have to accept. **Trust in your provider is an inherent part of any VPN scheme**, and no technology can remove that element from the equation.

## The Good News

But there is good news.

Even if your traffic is intercepted — reading it is generally impossible.

The best analogy I know: imagine having a conversation in a crowded café — but in a language nobody else around you speaks. Everyone can hear you. Nobody can understand a word. That's exactly how modern encryption works: data travels across the network in the open — but encrypted, and without the decryption key it's completely useless.

A couple of decades ago, data really was transmitted in plaintext — like speaking aloud in a language everyone understands, where intercepting anything was trivially easy. Today the picture is fundamentally different. **End-to-end encryption** has become the de facto standard: data is encrypted between your browser or app and the destination server, and no intermediate node can read its contents. As of 2024, over **95% of all web traffic** is transmitted via TLS/HTTPS — back in 2015, that figure was below 40%. One of the quietest yet most significant shifts in the history of the internet.

Your bank, your messenger, your streaming service — they all encrypt traffic before sending it out. And browsers don't just support secure mode anymore — they practically force you into it. Try visiting an unencrypted site: your browser will warn you several times in a row, then still require you to explicitly confirm you understand what you're doing.

The technology behind all of this is **TLS** (Transport Layer Security). It's the protocol for cryptographic data protection in transit — the one behind the padlock in your address bar, behind HTTPS, behind the fact that your passwords and card numbers don't fly across the network in plaintext.

## How Our Network Works

Our network runs on the **VLESS** protocol — and here's the key thing to understand.

There are protocols with heavier cryptography: they offer theoretically stronger resistance to cracking. But they come with a downside — they're **visible**. Their traffic has specific signatures that DPI (Deep Packet Inspection) systems at the provider level can detect and extract from the general stream. The stronger the cryptography — the more visible the traffic.

**VLESS** uses the same encryption principles as ordinary internet traffic — TLS. This is precisely what makes it indistinguishable from standard HTTPS. To an outside observer analyzing your traffic, you've been browsing some perfectly ordinary website for hours — and it's that site's security certificate providing the TLS encryption of your data (be prepared to explain why you're so obsessed with cat-girls).

We consciously choose **not maximum cryptographic complexity**, but **invisibility and camouflage**. You can't crack what doesn't exist — and in our philosophy that matters more than a heavier but visible cipher. Especially in conditions where DPI systems are growing smarter and more aggressive every year.

I wrote in detail about why this matters [here]({{< ref "blog/2026/02/20/паттерны-убивают" >}}): what actually kills VPN services — not protocol cracking, but detection through behavioral patterns.

## On Guarantees

And finally. The most important part.

If someone offers you a VPN service and talks about its reliability and "bulletproof" nature — **don't believe them**. Not that person, not that organization. In current conditions, nobody can guarantee this. Under a targeted regulatory attack on a specific service — nobody will hold out. The only real way to reduce that risk is to stay invisible. That's exactly what we're betting on.

Looking at the market overall — I think we're in for a serious consolidation. That doesn't mean services will disappear as a category: demand will only grow, and supply will follow. But the stability of large commercial services will likely get noticeably worse — and I wouldn't advise signing long-term contracts with anyone in this market. Too many precedents of services vanishing overnight, taking users' prepaid subscriptions with them.

What's actively discussed in the community: something like a "state VPN" might emerge — for accessing services that aren't technically banned, but can't function properly under current restrictions. A more pragmatic scenario seems likely to me: a series of deals between major players and the state, where tolerance for a service's existence is exchanged for cooperation. Semi-official arrangements in a gray zone — the designation of "authorized operators."

Either way, the overall vector here aligns with what we've already seen in telecom, banking, and media: **consolidation, the squeezing out of small players, and the establishment of state control over a few large ones**. A familiar story. The ending is predictable.
