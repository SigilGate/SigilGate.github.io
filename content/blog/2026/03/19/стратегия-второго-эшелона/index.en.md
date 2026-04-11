---
title: "Second-Tier Strategy"
slug: "second-tier-strategy"
date: 2026-03-19T00:00:00+00:00
draft: false
summary: "The fire is out — but the aftertaste remains. I reflect on hosting selection criteria: why I avoid European hosting, and why my choice is fourth-tier providers from third-world countries."
categories: ["Архитектура и исследования"]
rubrics: ["Перекрестки"]
tags: ["new", "хостинг", "VPS", "Япония", "блокировки"]
---

The fire is out. A [temporary solution has been found], the burning connectivity issue is closed. Seems like time to breathe.

I know the classic line: there's nothing more permanent than a temporary fix. But plugging holes is exactly what I'm trying to avoid. Which means it's time to sort out the hosting situation. And while I've been studying the market, something resembling a strategy has taken shape.

---

## Not Just Geography — But Freedom

The obvious choice seems clear: Western European countries are closest to central Russia — minimum ping, end of story.

Practice, however, has surprises in store. When you access the internet through European servers, you discover: the resources blocked by Russia's regulator open up — but simultaneously you lose access to resources blocked by European regulators. The level of European censorship, in my experience, is no inferior to the efforts of the domestic agency. It just blocks different things — but the overall feeling is that you've traded one set of restrictions for another.

My conclusion: geography alone isn't enough — what matters is the **level of internet freedom**. The recognized leaders on this metric are **Iceland**, **Estonia**, **Japan**, **Canada**, and the **USA**.

At the opposite end — Russia, China, Iran, North Korea: these countries automatically drop out of any consideration.

Japan has long been my primary reference point: the closest country to us by time zone with genuinely free internet. That's where I placed my bet.

---

## Second-Tier Providers

The second criterion — no less important — is the provider's level of name recognition. My approach here is somewhat paradoxical.

I deliberately choose **not the largest and most well-known hosters**.

The logic is simple: the IP address ranges of companies like **Amazon AWS**, **Cloudflare**, and **DigitalOcean** are known by heart — and they're the first to get blocked. Second-tier hosters — smaller companies, without a big name — don't attract such close attention from regulators. Which means they hold out a little longer.

Time has confirmed the validity of this strategy. Large providers with heavy traffic were the first to get blocked. But after the events of late last autumn, the wave caught the second tier as well.

---

## A Stalemate

For several years my hoster was a small American company with servers in Japan. The setup worked — though in recent years there were complications with money transfers for payment. In the last wave of blockings, their networks got caught in the sweep too: reaching the servers even via `ssh` from the Russian segment became impossible.

The hoster's website also became inaccessible — though not directly. The issue was restrictions imposed against **Cloudflare**: a significant portion of the internet using its **DNS** and **CDN** became unavailable along with it. The result was a stalemate:

> you need a VPN to configure the VPN, to reach the control panel of the server running the VPN.

This should have been resolved long ago.

---

## The Japanese Market: Observations

Since the strategy implies shifting toward even more local, regional hosters — I conducted a small study of the Japanese VPS market. The detailed results are in the [appendix](#appendix-vps-market-research-in-japan) below.

In brief: the Japanese hosting market is **heavily regionalized**. Far from all providers have even an English interface. Far from all accept international payment methods. The market for pre-installed software — WordPress, office solutions — is well developed; bare VPS rental is substantially worse. The list of providers that work with a clean server isn't very long.

And it gets even shorter, because some hosters don't work with clients from Russia — cutting them off by IP address at the registration stage. I seem to have exhausted the available options.

---

## Next Step

It's time to expand the geography. Next up — hosters from the **Asia-Pacific region**: **Singapore**, **South Korea**, **Taiwan**. Latency will increase slightly — but the range of options will be wider.

---

## Appendix: VPS Market Research in Japan

*Research conducted in December 2025, during the project's MVP phase. The market changes — specific prices and terms should be verified for current accuracy.*

### Hosting Requirements

For placing an Entry node — the front-line node accepting client connections via VLESS/XHTTP and masquerading as a legitimate HTTPS site — the minimum requirements look like this:

| Parameter | Requirement |
|-----------|-------------|
| **Service type** | VPS / VDS with root access |
| **OS** | Debian 11/12 or Ubuntu 22.04/24.04 |
| **vCPU** | 1 core |
| **RAM** | 1 GB |
| **Disk** | 10 GB SSD |
| **Traffic** | 1–2 TB / month |
| **IPv4** | Dedicated public address |
| **Payment** | USD or cryptocurrency |
| **Budget** | Up to $10 / month |

Managed hosting, PaaS platforms, and shared hosting are excluded: without root OS access, it won't work.

---

### Japanese Local Providers

#### ConoHa (GMO Internet Group)

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo |
| **Minimum plan** | 1 GB RAM, 2 vCPU, 50 GB SSD — ¥900/mo (~$6) |
| **Billing** | Monthly + hourly |
| **Payment** | Credit cards, PayPal |
| **Interface** | English + Japanese |

Part of the largest Japanese hosting group GMO. Control panel in English — notifications come only in Japanese. Support also primarily in Japanese. Good reputation, stable operation.

---

#### ABLENET

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo |
| **Minimum plan** | ~$8–10/mo |
| **Billing** | Monthly / quarterly / annual |
| **Payment** | Credit cards |
| **Interface** | English |

One of the oldest Japanese hosters. English website and control panel. Unlimited traffic — a significant advantage. Trial period available.

---

#### Sakura Internet

| Parameter | Value |
|-----------|-------|
| **Data centers** | Tokyo, Osaka, Hokkaido |
| **Minimum plan** | 1 GB RAM — ¥935/mo (~$6–7) |
| **Billing** | Monthly / annual |
| **Payment** | Credit cards |
| **Interface** | Japanese only |

Since 1996, with its own data center in Ishikari. Two-week free trial. High reliability — but website and support in Japanese only; a translator is mandatory.

---

#### Kagoya Japan

| Parameter | Value |
|-----------|-------|
| **Data center** | Kyoto |
| **Minimum plan** | From ¥550/mo (~$4) |
| **Billing** | Monthly |
| **Interface** | Japanese (partial English) |

Competitive pricing, focused on the Japanese market. Virtually unknown outside Japan — which is, in fact, the advantage.

---

#### XServer VPS

| Parameter | Value |
|-----------|-------|
| **Data center** | Japan |
| **Interface** | Japanese only |

Major provider since 2004, NVMe SSD, KVM. Website and support in Japanese only — extremely inconvenient without knowledge of the language.

---

### International Providers with Data Centers in Japan

#### Vultr

| Parameter | Value |
|-----------|-------|
| **Data centers** | Tokyo, Osaka |
| **Minimum plan** | 1 GB RAM, 1 vCPU, 25 GB SSD — $5/mo |
| **Billing** | Hourly ($0.007/hr) |
| **Payment** | USD; cards, PayPal, Bitcoin |
| **Traffic** | 1 TB/mo |

One of the best options for a quick start. Hourly billing — you can spin up and delete a node without overpaying. Good documentation, API, instant deployment.

---

#### Linode (Akamai)

| Parameter | Value |
|-----------|-------|
| **Data centers** | Tokyo, Osaka |
| **Minimum plan** | 1 GB RAM, 1 vCPU, 25 GB SSD — $5/mo |
| **Billing** | Hourly ($0.0075/hr) |
| **Payment** | USD; cards, PayPal |
| **Traffic** | 1 TB/mo |

A time-tested provider — since 2003. After merging with Akamai, it gained an expanded network. Does not accept cryptocurrency.

---

#### LightNode

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo |
| **Minimum plan** | 1 GB RAM — $7.71/mo |
| **Billing** | Hourly ($0.012/hr) |
| **Payment** | USD; cards, BTC, ETH, USDT, SOL and others |

Wide cryptocurrency selection — the main advantage. Less well-known, actively developing.

---

#### HostDare

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo (Xtom) |
| **Minimum plan** | 768 MB RAM, 10 GB NVMe — $25.99/year (~$2.17/mo) |
| **Billing** | Semi-annual / annual |
| **Payment** | PayPal, Alipay, cryptocurrency |
| **Traffic** | 250–500 GB/mo |

The cheapest option for Japan. Limited traffic on budget plans — better suited for proof-of-concept testing.

---

#### Contabo

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo |
| **Minimum plan** | 4 vCPU, 8 GB RAM, 75 GB NVMe — from $7/mo |
| **Billing** | Monthly |
| **Payment** | USD; cards, PayPal |
| **Traffic** | Unlimited |

A German company with resources excessive by MVP standards — but an excellent price-to-performance ratio. Unlimited traffic. No hourly billing.

---

#### Kamatera

| Parameter | Value |
|-----------|-------|
| **Data center** | Tokyo |
| **Minimum plan** | Flexible configuration from ~$4/mo |
| **Billing** | Hourly |
| **Payment** | USD; cards |

Flexible resource configurator, hourly billing, 24/7 support.

---

### Comparison Table

| Provider | Min. price | Billing | Crypto | Traffic | Interface |
|----------|-----------|---------|--------|---------|-----------|
| **Vultr** | $5/mo | Hourly | ✅ BTC | 1 TB | EN |
| **Linode** | $5/mo | Hourly | ❌ | 1 TB | EN |
| **Kamatera** | ~$4/mo | Hourly | ❌ | — | EN |
| **ConoHa** | ~$6/mo | Hourly | ❌ | — | EN/JP |
| **LightNode** | $7.71/mo | Hourly | ✅ Many | — | EN |
| **Contabo** | $7/mo | Monthly | ❌ | ∞ | EN |
| **ABLENET** | ~$8/mo | Monthly | ❌ | ∞ | EN |
| **HostDare** | $2.17/mo | Annual | ✅ | 250 GB | EN |
| **Kagoya** | ~$4/mo | Monthly | ❌ | — | JP |
| **Sakura** | ~$6/mo | Monthly | ❌ | — | JP |

---

### Recommendations

**Top 3 for a quick start:**

1. **Vultr** — optimal balance of price, convenience, and flexibility: hourly billing, Bitcoin, instant deployment.
2. **Linode (Akamai)** — proven reliability, part of the Akamai network; comparable to Vultr in parameters, without cryptocurrency.
3. **LightNode** — if crypto payment is essential: wide coin selection, hourly billing, slightly more expensive.

**Budget option for testing:** **HostDare** — $25.99/year, suitable for proof of concept.
