---
title: "Utopia"
slug: "utopia"
date: 2026-03-30T00:00:00+00:00
draft: false
summary: "Why a private network is first and foremost a social structure, how it grows from the bottom up, and why freedom of communication should be a norm — not a privilege."
categories: ["Technology & Society"]
rubrics: ["Invisible Threads"]
tags: ["architecture", "computer-networks"]
---

Thomas More wrote *Utopia* — not because he believed in its literal realization, but because he thought: without an image of the desired future, there is no direction to move. Tommaso Campanella wrote *The City of the Sun* for much the same reason — from the dungeons of the Inquisition. Kropotkin developed his theory of mutual aid in Siberian exile. Many people, in far worse circumstances than mine, kept thinking about what the world could be. Or — its better copy.

In the [previous part]({{< ref "blog/2026/03/12/network-principles" >}}) I described the principles — the architectural philosophy that networks resistant to detection should follow. Today I want to go further: to sketch what it might look like for these principles to actually be realized. A utopia in the original sense: "a place that doesn't exist" — but one that sets the direction. To think out loud about what private networks of the future might look like, and what seeds of that future are already visible today.

---

If you came here looking for a ready-made recipe for building a secure, private, detection-resistant network — I have to disappoint you. There isn't one.

The war between shield and sword has no finish line. Blocking tools evolve — and circumvention tools evolve with them. DPI gets smarter, learns to recognize traffic by behavioral patterns, entropy, timing characteristics. In response, new transports appear, new masking methods, new ways of mimicking legitimate traffic. This is not a race with a finish line — it's the eternal run of life.

There is no silver bullet, and searching for one is pointless.

All I have is a basic understanding of how computer networks work. Everything I write about in these articles is not secret knowledge, not insider information, not the result of classified research. It's what you can read in any textbook — Tanenbaum's *Computer Networks*, for instance.

Concrete implementations exist. There is accumulated experience, there are tools, there are communities of people who know and apply all of this. But a tool in the hands of someone who doesn't understand the principles — that's just a crutch that works today and breaks tomorrow when conditions change. Only this understanding — not a set of ready-made instructions — allows you to adapt to constantly shifting rules.

But even technical knowledge is secondary.

I understand this will sound strange in a blog dedicated to protocols, transports, and architectural decisions. But here's the thing: the organizational side of such networks matters more than the technical. The true, fundamental nature of computer networks is social. Always.

For a packet to travel from point A to point B across a national border, through a filter, through a restriction — there has to be someone on the other side willing to receive it. A node. A person. A connection. Networks don't rest on protocols — they rest on trust between people who decided to build them. A protocol is a language. But before you speak, someone has to want to listen.

Behind the network layer and the technical infrastructure lies something larger: mechanisms of free exchange — of information, knowledge, money, goods, and services. Through social networks. Through personal connections. By phone. Sometimes by mail or railway, for that matter. In the 1980s, Soviet engineers read foreign technical journals through interlibrary exchange systems and through personal contacts with colleagues from other socialist countries. That was literally a social network built above the Iron Curtain. When it worked — knowledge flowed. When it broke — the gap appeared.

As long as those connections are alive, you'll manage to set up a technical channel. That's a matter of tools, and tools can be found. If the connections disappear — you'll find yourself locked inside one region, one society, one culture. And then no protocol will help.

And it gets worse. If you can't see the world beyond your bubble, you're left with only one version of the truth. The reference point vanishes. Without it, critical thinking becomes impossible — not because you're slow, but because you have no other picture to find the ten differences in. Meanwhile, the world keeps running forward. It doesn't wait. And to stay in place, you have to run — as hard as you can.

So here is the paradox I want to state clearly: building technological sovereignty by restricting the flow of information is the surest path to losing that sovereignty. Isolation doesn't protect. It leads to falling behind.

That's why the core narrative of **Sigil Gate** is about building connections. Digging tunnels, building bridges, connecting countries and continents.

The whole story of how **Sigil Gate** came to be is a story of personal connections. Friends whose services got blocked. Acquaintances who needed a reliable channel. People from my closest circle, bound to me by years of shared work and daily contact. The project didn't grow out of ideology — it grew out of the concrete needs of concrete people. And in that sense, the entire history of the project is, in many ways, my personal history.

Yesterday someone wrote to me in total enthusiasm about how I "got this whole thing going," and started talking about how great it would be if all people united and pushed back against the blocks together, armed with our tools. I understand the feeling. But I'm talking about something else. I'm not calling for world revolution.

I'm talking about a tunnel that works. Because on the other side of that tunnel is a person who maintains it. About the fact that everything in this world runs on string and duct tape — and also on trust, personal connections, sympathy, and years of contact.

Today I added another user to our network. Not a plus on the counter. A specific person — fifteen years of acquaintance, shared work in two organizations, daily communication. And now all members of that small group are inside the network. I consider this a personal victory. Not because it gives me statistics — but because it matters to me personally.

---

Why do I write about this? Because I believe: at the foundation of everything is the free association of a small group of people. Informal. Voluntary. Based not on a legally binding contract or corporate policy, but on simple human trust.

Kropotkin called this mutual aid — and saw it not as altruism or idealism, but as an evolutionary fact. People unite not because they're forced to, but because surviving together is easier. He studied Russian artels: self-organizing workers' associations with no boss, just a common purpose and shared responsibility. No charter, no registration — just an agreement between people who knew each other by name. Bakunin built his theory of federation on the same foundation: not top-down, but bottom-up. First — the free association of people. Then — the free association of such associations.

The **first tier** of the network looks exactly like this. A few people who know and trust each other. Family. Close friends. Colleagues. People you drink coffee with and talk about what's happening in the world. One node, a handful of clients, one person who set it all up and keeps it running. No bureaucracy, no registration, no terms of service. Just: "I set this up, use it." Thousands of such networks exist right now, and most of them have no name.

**Sigil Gate** was conceived precisely as a tool for this tier. Not a global service, not a platform with millions of users — just a set of tools that lets people communicate freely. Message each other on social networks. Make calls. Send messages — regardless of whether you're discussing what's for breakfast or staying in touch with family that moved abroad. The architecture was built from the start to handle a few dozen nodes. Not for lack of ambition — but because this scale matches the nature of what we're building: a network of equal participants, connected by personal acquaintance and trust built on lived experience.

I believe that these kinds of associations — small, voluntary, grounded in trust — will form the foundation of the free networks of the future. Not corporations, not state structures, not centralized platforms. Small associations of small social groups.

The question is different: what would it take to make this real for people who don't know their way around servers and protocols?

Very little. Simple, accessible tools. Easy deployment: standing up a node should be no harder than setting up a router. User and node management without digging into config files. Automation of routine tasks — updates, rotation, monitoring — everything that currently demands constant administrator attention. Adaptability: the network must be able to reconfigure itself when a node goes down or becomes unreachable. And — most importantly — the ability to rebuild from scratch quickly, without losing information about nodes, users, and settings. For when you temporarily lose access to the infrastructure and have to build new.

The goal of Sigil Gate is to scale the experience of one small community to another. So that a person who once built a network for their friends and family can, without great effort, help build another one — in a different city, a different country, a different community. Without transferring control, without creating dependency — by transferring a tool.

That's the technical agenda we're working on. Not inventing a new protocol — but lowering the barrier to entry until trust and willingness to help become sufficient conditions.

The **second tier** is a community. A small organization. Some structure becomes necessary here: who administers, who pays for servers, how new participants are added. But the principle is the same — trust first, technology second. A network that expands not because an ad campaign ran, but because someone told someone else: "This works, give it a try."

And beyond that — free cooperation between such associations. Independent networks that agree to exchange traffic. Each maintains its autonomy, managed by its own people, living by its own rules. But a packet leaving one network can pass through another — because there are also people there who care. Resilience that grows not through centralization, but through the number of such connections.

Pierre-Joseph Proudhon called this mutualism — free exchange between self-governing associations, based on reciprocity, not profit or coercion. In *The Principle of Federation* (1863) he described federation as the only form of organization in which cooperation doesn't destroy autonomy: associations agree on specific things — and only those things — remaining independent in everything else. Not a hierarchy and not a merger, but a network of agreements between equals.

He was writing about political society. But he could have been writing about how a private network should be organized.

And you know what? These principles are already implemented. They're working right now — we just don't always call them by name.

The Tor network runs on thousands of volunteer operators around the world who run nodes on their own servers. No corporation, no center that can be decapitated. Each node is autonomous. Resilience through the number of participants and their geographic spread. This is mutualism in its purest form: everyone contributes a resource, everyone gains access to the commons.

Mesh networks go further. Every device in such a network is simultaneously a client and a routing node. No dedicated server, no central control point. A packet finds its own way through neighbors — through people who happened to be nearby and agreed to participate. These networks were deployed during the protests in Hong Kong, after earthquakes when infrastructure collapsed. They work precisely when centralized systems stop working.

The principle is one: resilience through decentralization. Bakunin wrote about this in 1873. Engineers implemented it in code at the end of the 20th century. We're building on the same foundation.

---

These currents always resemble a waterfall. First — a thin trickle. Then streams join into rivers. And then the flow can no longer be held back by any dam. History has many examples of what started in a basement or in correspondence between a dozen people and ultimately changed the world — sometimes quickly, sometimes across a generation, but unfailingly.

I'm not trying to save the world. I'm not building a global revolution. I'm making products that are needed here and now. For specific people, for specific problems, with real value in a specific moment in time. Today — that's a few dozen users who can communicate freely. A person who watches what they want to watch. A family separated by a border that can call each other without interference.

But I believe that what I'm doing makes the world a little better. Makes the world around me a little closer to a world where freedom of communication is a norm — not a privilege.
