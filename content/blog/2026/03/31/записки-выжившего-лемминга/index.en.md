---
title: "Notes of a Surviving Lemming"
slug: "notes-of-a-surviving-lemming"
date: 2026-03-31T00:00:00+00:00
draft: false
summary: "Last autumn I lost my VPN. A story about the patterns that kill services, the arms race between regulators and protocols — and why all of it is ultimately about people."
categories: ["Technology & Society"]
rubrics: ["Invisible Threads"]
tags: ["DPI", "VLESS", "architecture", "security"]
---

## [Part 1. Patterns Kill](https://habr.com/ru/articles/1017296/)

Last autumn I lost my VPN service. A classic self-hosted setup on a VDS rented from a foreign provider, no middlemen. It had worked reliably, cost next to nothing — and I'd grown used to it just working. The autumn wave of blocks swept it away along with thousands of others like it.

I wasn't the only one caught in the blast. The wave hit both solo setups and major commercial services. The anxiety was compounded by the fact that the situation varied wildly depending on region, provider, and connection type — which added to the chaos and made any generalization nearly impossible.

The November collapse was a shock. VLESS — a protocol that had seemed unbreakable — fell. The media presented it as a great achievement of the Russian regulator: Russia had accomplished what China could not — the country with the world's most sophisticated internet censorship system, years of experience, and enormous resources. The Great Firewall, for all its power, had never found a countermeasure to VLESS. The Russian regulator did.

But when I started piecing together what had actually happened, the picture was less grim. The protocol held. No one has cracked it to this day. The problem wasn't the protocol — it was the people using it. They were all brought down by **shared patterns**: characteristic traffic signatures that DPI systems had learned to detect and isolate from the general flow.

VLESS had become enormously popular — and that, I now understand, was the first warning sign I missed. Popularity brought convenience and tooling: web panels, ready-made scripts, tools for every taste. I was one of the ten million lemmings who can't all be wrong. I got swept up in the collective euphoria, came to believe in the invulnerability of VLESS — and completely let my guard down. I installed a convenient 3X-UI panel without bothering to change the default port or hide it in any way.

That's exactly what DPI systems were detecting. Millions of users following the same template: identical setup guides, the same traffic-mimicry sites, the same tools and panels with default ports — which, as it turned out, are known not only to enthusiasts. On top of that, active probing was identifying the characteristic deviation in the handshake.

The result of the regulator's campaign wasn't a compromise of the protocol — it was the identification and compromise of nodes using it, followed by IP-based blocking. I confirmed this personally: in my frantic attempt to restore the fallen service, I lost SSH access to the node — but only from the Russian segment. From foreign servers, it was still reachable.

The VLESS blocking campaign was unprecedented in scale. It made one thing clear: the days of relying entirely on a single technology — even the best one — are over. No one can feel invulnerable anymore.

But the story has a flip side. The campaign exposed the regulator's own weaknesses. When all resources were thrown at VLESS, many other protocols that had been blocked before suddenly started working again. For a short time, previously inaccessible resources became reachable — the hardware, apparently, couldn't handle blocking everything at once: total control ran into the limits of the equipment.

Still, the main conclusion drawn by most market participants was unambiguous: life would never be the same. The schemes that worked before — rent a server abroad for the price of a coffee, run a secure tunnel to it, get your personal VPN — no longer work. Such channels will be identified and blocked. And as DPI systems and techniques develop, the time between deployment and discovery will only shrink.

The era of simple schemes is over. Stable operation in the future will require more complex infrastructural solutions — large-scale and diversified, capable of responding flexibly to regulatory action: changing topology and protocols as the situation demands, and providing ever-deeper masking.

This story made me stop and think. Not about how to quickly restore the lost service — technically that's a solvable problem. But about the nature of what happened: why did a scheme that worked for years suddenly break?

---

## [Part 2. Arms Race](https://habr.com/ru/articles/1017502/)

Somewhere in mid-2018, an article titled ["DPI-Resistant Tunnels and VPNs"](https://habr.com/ru/articles/415977/) appeared on Habr. It was one of the first articles where I started learning about the topic. A solid overview of solutions, arguments for and against running your own VPN. In those years I was just starting to build my first private networks. The tool choices were straightforward:

- **OpenVPN** — the main "people's VPN" of the era. Reliable, flexible, well-documented. But heavy — and recognizable: its characteristic TLS handshake and traffic patterns made it visible to any DPI that wanted to see it.
- **SSH forwarding** — the other extreme: a minimalist tunnel over SSH, mimicking administrative traffic. Fine for specific tasks, not for daily use.
- **Shadowsocks** appeared as a direct answer to the Great Firewall: a proxy masquerading as an encrypted stream with no obvious protocol signature, built specifically to counter DPI — and for a long time it did the job.
- **WireGuard** stood out from the rest: simple, fast, elegant — but transparent. No obfuscation, traffic identified instantly. Speed in exchange for stealth.

I eventually settled on **IKEv2** — a corporate standard, unpopular among enthusiasts. That's precisely why it stayed below the radar for so long: no mass adoption — no consistent patterns — no priority for blocking.

But what was truly remarkable was something else: the author tried to look into the future. A thankless task — making predictions — but many of that article's forecasts came true. The author pointed to a trend that was only beginning to take shape: encryption alone was no longer enough. The next frontier was **obfuscation**: traffic had to become not just encrypted, but unclassifiable, indistinguishable from any known protocol. And beyond that horizon, the author predicted the emergence of **mimicry** protocols: VPNs pretending to be ordinary web servers. Knock the wrong way — you get a page with cats. Knock the right way — you get a tunnel. A few years later, that's exactly what we got.

The pioneers were **VMESS**, and then **VLESS**. Their shared principle, and that of the analogues that followed, is: TLS encryption, minimal headers, mimicry of a live HTTPS host, a "friend or foe" system at the handshake level. Fallback to a real web server — for anyone who knocked wrong. No TLS-over-TLS or other heavy schemes that themselves become a de-anonymizing signature.

My introduction to **VLESS** happened in its first months of existence — coincidence of timing. When I set up my first Xray server, the protocol had just launched and client support could be counted on one hand. No web panels or other convenience layers: configs were written by hand in the terminal, directly on the server. That's how it ran for years, with minimal attention from me, rarely needing more than a restart.

The protocols proved so effective that major commercial VPN services one by one began adopting them, or building their own solutions on the same underlying principles.

Where we stand now: breaking the best of the modern protocols still hasn't been achieved. But the search is in its active phase. And the main line of attack chosen by regulators is not cryptographic. It's the identification of **traffic behavior patterns**: characteristic signatures that allow a node to be identified and blocked precisely. Sometimes "precisely" means entire subnets or taking down major hosting providers. But the principle is the same: don't break the lock — learn to recognize the right door.

Blocking tools evolve — and circumvention tools evolve with them. DPI gets smarter, learns to recognize traffic by behavioral patterns, entropy, timing. In response, new transports appear, new masking methods, new ways of mimicking legitimate traffic. This isn't a race with a finish line — it's the eternal run of life. The pace will only accelerate. Those who can't adapt will drop out.

For the market as a whole — I think we're in for a serious consolidation. That doesn't mean services will disappear as a category: demand will only grow, and where there's demand there'll be supply. But the stability of large commercial services will likely deteriorate noticeably — and I wouldn't advise signing long-term contracts with anyone in this market. Too many precedents of services disappearing overnight along with their customers' prepaid subscriptions.

Among the things actively discussed in the community: something like a "state VPN" may emerge — for accessing services that are de facto not banned but can't function normally under current restrictions. A more pragmatic scenario seems to me: a series of deals between major players and the state, in exchange for cooperation. Semi-official arrangements in a grey zone — designated "authorized operators."

In any case, the general vector here aligns with what we've already seen in telecom, banking, and media: **consolidation, the displacement of smaller players, and the establishment of state control over a few large ones**. A familiar story. The ending is predictable.

---

## [Part 3. The Challenge of the Times](https://habr.com/ru/articles/1017776/)

If you're reading this looking for a ready-made recipe for building a secure, private, detection-resistant network — I have to disappoint you. I don't have one. Any recipe is useless in the hands of someone who doesn't understand the principles — it's just a crutch that works today and breaks tomorrow when conditions change.

All I have is a basic understanding of how computer networks work. Everything I write about in these articles is not secret knowledge, not insider information, not the result of classified research. All of it can be read in any computer networking textbook — right now, in any bookstore. Or in primary sources on GitHub. There is accumulated experience from specific implementations, there are tools, there are communities of people who love this, know it, practice it. And only that allows adapting to constantly shifting rules.

There is no silver bullet, and searching for one is pointless. The central challenge, in my view, is not finding a solution that works today, or the right protocol.

Technical solutions change constantly. Protocols will appear — each better than the last — and disappear. Protocols with heavy cryptography are fading — simply because they're visible. The stronger the cryptography, the more conspicuous the traffic. The trend in building private networks runs not toward maximum cryptographic complexity, but toward invisibility and masking.

Wrapping traffic in a tunnel is no longer enough. Modern DPI systems look at shape. Header size, transmission rhythm, handshake character — all of this adds up to a digital fingerprint that identifies traffic as confidently as a person by their gait.

The challenge of our time is to make a connection statistically invisible. Indistinguishable from background traffic by volume, rhythm, or behavior. If DPI has learned to recognize the characteristic rhythm of VPN traffic — you need to learn to break that rhythm. If it can isolate anomalies in the flow — you need to reduce deviations to the level of statistical noise, indistinguishable from background HTTPS.

A few basic principles can help.

**No VPN patterns.** Any protocols at the transport or application layer that create characteristic patterns — from the dinosaurs like OpenVPN to relatively fresh and exotic protocols like Hysteria 2 — stand out too much against the general background.

**Consistency is a target.** Even perfectly masked traffic becomes vulnerable if it regularly arrives from the same address. The answer is continuous rotation: automatic cycling of nodes, endpoints, and domains.

**Layer isolation.** Compromising one element shouldn't pull everything else with it. At minimum four independent layers: network, operational, service, commercial. A single service where everything is concentrated is a single entry point for an attacker.

**Right sizing.** The optimal network size is a balance: large enough to enable full rotation; small enough to stay below the detection threshold. Size is a pattern too.

This architectural philosophy centers on one simple idea: the best way to hide something is to put it in plain sight.

The best place for a person to hide is in a crowd. Be like everyone else. Look like everyone else. Move like everyone else. Leave nothing to grab onto.

Digital invisibility rests on the same principles.

---

## [Part 4. The Last Stand](https://habr.com/ru/articles/1017786/)

I understand this will sound maximally strange in a text devoted to protocols and architectural solutions. But here's the thing: the organizational side of such networks matters more than the technical. The true, fundamental nature of computer networks is social. Always.

For a packet to travel from point A to point B across a national border — through a barrier, through a filter, through a restriction — there has to be someone on the other side willing to receive it. A node. A person. A connection. Networks don't rest on protocols — they rest on trust between people who decided to build one. A protocol is a language. But before you speak, someone has to want to listen.

In the 1980s, Soviet engineers read foreign technical journals through interlibrary exchange systems and through personal contacts with colleagues from other socialist countries. That was literally a social network built above the Iron Curtain. When it worked — knowledge flowed. When it broke — the gap appeared.

As long as those connections are alive, you'll manage to set up a technical channel. That's a matter of tools, and tools can be found. If the connections disappear — you'll find yourself locked inside one region, one society, one culture. And then no protocol will help.

So here is the paradox I want to state clearly: building technological sovereignty by restricting the flow of information is the surest path to losing that sovereignty. Isolation doesn't protect. It leads to falling behind.

The whole story of my work building private networks has always been about personal stories that grew from the concrete needs of concrete people. Relatives who ended up on the other side of a border. Friends whose service got blocked. Acquaintances who needed a reliable channel. Kids who want to watch their favorite YouTube creators.

Today I added another user to our network. Not a plus on the counter. A specific person — fifteen years of acquaintance, shared work in two organizations, daily communication. And now all the members of that small group are inside the network. I consider this a personal victory. Not because it gives me statistics — but because it matters to me personally.

In my articles I write about a tunnel that works. Because on the other side of that tunnel is a person who maintains it. About the fact that everything in this world runs on string and duct tape. And also on trust, personal connections, sympathy, and years of contact.

These currents always resemble a waterfall. First — a thin trickle. Then streams join into rivers. And then the flow can no longer be held back by any dam. History has many examples of what started in a basement or in correspondence between a dozen people and ultimately led to global consequences.

But I'm not trying to save the world. I'm not trying to accelerate the victory of a world revolution. I make products that are needed here and now. For specific people, for specific problems, with real value in a specific moment in time. To watch the anime you love. To communicate without interference through a messenger that for some reason stopped working in Russia.

I believe it's precisely these kinds of associations that will form the foundation of the free networks of the future. States, corporations, and centralized platforms will topple — and only small associations of small social groups will survive.

I will believe until my last heartbeat that what I'm doing makes the world a little better. Makes the world around me a little closer to a world where freedom of communication is a norm — not a privilege.
