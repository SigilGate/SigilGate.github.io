---
title: "Economics Over Coffee"
slug: "economics-over-coffee"
date: 2026-02-21T00:00:00+00:00
draft: false
summary: "Our financial operator announced an account freeze — so I decided to honestly calculate what the project actually costs. Spoiler: cheaper than it seems."
categories: ["Project Diary"]
rubrics: ["Margin Notes"]
tags: ["economics", "infrastructure"]
---

This morning brought some grim news — our project may be heading for financial trouble. The holder of our critical infrastructure, the owner of all the organization's foreign assets and its financial operator, announced an upcoming account freeze for an indefinite period.

A perfect time to think a little about economics — over a morning cup of coffee.

Economics is always about money. We use a complex payment system: payment chains travel a long way, and paying for services remains one of the most critically vulnerable points in our network. We buy domain names in bulk — dirt cheap — in rubles; pay for AI tools in dollars; pay for hosting in yen. Somewhere across the ocean, servers are still running that were paid for with a lucky crypto investment — in bitcoin. Payments go through tenge, via a Kazakh offshore, or through crypto exchange operations — we've repeatedly eaten exchange fees and currency spreads that, in the end, could add up to the lion's share of the final service cost.

To avoid drowning in this chaos, I decided to simplify and reduce everything to a single denominator. In 1986, *The Economist* invented the "Big Mac Index" — a simple way to compare purchasing power across countries using the price of one identical product. I'll borrow that principle, but choose a different product: **a cup of coffee**. Let's say it costs about 300 rubles (or $3). When costs aren't a clean multiple of that amount, I'll round up to the nearest whole cup, treating the difference as overhead: operating costs, fees, exchange rate spreads, and so on.

As the founding father of economics, William Petty, used to say — "Let us calculate!"

## Domains

I bought a batch of domain names for the necodate cover site across various TLDs. The year came to 6–7 cups of coffee — about half a cup per month, but I'm rounding up to one.

Right now I have 5–7 domain names, and I know I won't stop there. First, I'm a bit of a shopaholic and can't resist a nice domain on sale. Second, I have an idea to bind users to domains rather than IP addresses: that way, rotating entry nodes becomes seamless — the client finds the Entry node via DNS, IP addresses change, and the user never notices. If the idea works, I'll need at least as many domains as Entry nodes.

**Total: 1 cup per month.**

## Hosting

Core nodes are more powerful — they run about **two cups of coffee** each. Entry nodes are roughly three times weaker and proportionally cheaper: **one cup**. One Core node easily serves three Entry nodes, maybe more — practice will tell. The minimum technically justified cluster: 1 Core and 3 Entry nodes, totaling **5 cups**.

For a full-scale network — with distributed storage and real decentralization — you need three such clusters: 3 Core nodes and 9 Entry nodes.

**Total: 15 cups per month.**

## Development

On permanent staff, we have our AI assistant — $20 a month, seven cups of coffee. You could argue whether this counts as an operating expense — but for now, the project moves largely thanks to it. Call it an investment in development.

**Total: 7 cups per month.**

Everything else is **free**. Everyone working on the project does it in their spare time, for the love of it. That's a contribution no money can measure — the only thing to say is thank you.

## Total Costs

**23 cups of coffee per month** — the cost of a minimal but fully functional deployment.

## The Other Side of the Ledger

Each Entry node comfortably handles three users. With occasional use — browsing, social media, streaming, mostly from mobile — that number can go up to five. Across 9 Entry nodes at minimum load, that's **27 people** without any degradation in connection quality.

27 people and 23 cups of coffee — you could even connect a few participants for free without going into the red.

The bottom line: **the cost of one connection — one cup of coffee**. Minimum topology — 3 Core and 9 Entry nodes. Minimum number of participants to break even — 30.

Two months to reach that number. The clock starts March 1st.
