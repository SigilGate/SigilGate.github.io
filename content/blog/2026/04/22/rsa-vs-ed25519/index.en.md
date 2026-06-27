---
title: "RSA vs Ed25519"
slug: "rsa-vs-ed25519"
date: 2026-04-22T00:00:00+00:00
draft: false
summary: "On conservatism, habits, and technologies that change faster than you can get used to them."
categories: ["Авторский взгляд"]
rubrics: ["Мысли вслух"]
tags: ["rsa", "ed25519", "хабр"]
---

In my last article about [systemd timer](https://habr.com/ru/articles/1025764/) I shared an eye-opening (for me) discovery and got a comment from @Xelld:

> "And will the next revelations be resolved, networkd and mounts? :)"

I have a good relationship with humor. You know, by the age of forty-five I even seem to have learned to recognize sarcasm :)

And you know what — yes. The world is full of new and wonderful words. Resolved, networkd and mounts. Over time, you get used to them and stop being surprised. These words become ordinary and mundane, they firmly enter your life and your fingers type them on the keyboard automatically, mechanically, without thinking.

But I don't stop being surprised — practically every day. Life around us changes and always brings something new and interesting. You just start getting used to the good things — and life gets even better! (Sign reading "SARCASM!!!", for those who haven't learned to recognize it yet.)

I am very conservative. I don't like changing my tools and familiar patterns of behavior. I like feeling stability around me, the confidence that the things I use are always in their place.

Unfortunately (or fortunately) — the world doesn't stand still. Everything flows, everything changes. I'm forced to accept this with a certain degree of fatalism.

Cron and systemd timer are just one of the small ice chips on top of this iceberg. A telling example of how familiar and simple things disappear, replaced by combine harvesters. With vastly more complex logic that demands cognitive effort to understand how it all works and how to make it run.

From the comments of that same article:

> In cron, a schedule is set with a single line I can write from memory. In systemd you need to create 2 files of 5–10 lines each, which I cannot write from memory. I'm constantly either Googling or going to ChatGPT.

To my dismay, there are a great many things like this, and their number only grows every day. I can't keep up with adapting to this constantly changing world. I'm too old for all of this. I'm a dinosaur, and evolution should rid the world of people like me.

I only just started getting used to Telegram — it only took 10–15 years. I mastered it well enough to start my own [channel](https://t.me/SigilGate) — and then Telegram got blocked a week later.

For five years I kept meaning to dive deeper into iptables — constantly configuring them by trial and error. And when I finally found the time — I discovered that iptables are obsolete. That's not how things are done anymore — they invented something called nftables. OK — now I have another five years of procrastination before I sit down to master yet another amazing tool. One more new word in an endless stream of new words.

Over time, new words stop surprising you. To reduce the chaos and uncertainty, you start fitting new words into the framework of your familiar concepts, realizing that only the wrappers and labels change — the underlying essences remain the same.

But I write my articles for those who haven't yet lost the ability to be surprised. For those whose world is young enough that some things still need to be pointed at — because they don't have a name yet.

Forgive my old-teacher's professional bias. I always explain things assuming zero prior knowledge. And I'm always glad when there are prepared students in the audience who don't need any explanation.

So, let's continue... Today's lecture topic: **"RSA vs Ed25519"**.

---

# RSA vs Ed25519

A few links to start:
- [Secure Secure Shell](https://blog.stribik.technology/2015/01/04/secure-secure-shell.html)
- [RSA vs. DSA for SSH authentication keys](https://security.stackexchange.com/questions/5096/rsa-vs-dsa-for-ssh-authentication-keys/46781#46781)
- [Understanding the different types of SSH keys](https://www.dev-notes.ru/articles/devops/understanding-the-different-types-of-ssh-key/)
- [SSH ed25519 keys vs RSA — Benefits and drawbacks? What do others use?](https://www.reddit.com/r/sysadmin/comments/4gktbr/ssh_ed25519_keys_vs_rsa_benefits_and_drawbacks/?tl=ru)
- [A tragic story. The RSA algorithm](https://habr.com/ru/articles/75193/)
- [RSA in simple words and pictures](https://habr.com/ru/articles/745820/)
- [Elliptic curve cryptography made accessible](https://habr.com/ru/articles/335906/)
- [Digital signature — Ed25519](https://ecp.sale/glossary/ed25519.html)
- [How I did NOT crack ED25519](https://habr.com/ru/articles/939686/)
- [Curve25519, EdDSA and Poly1305: Three overlooked cryptographic primitives](https://habr.com/ru/articles/247873/)
- [OpenSSH 6.5 release](https://www.opennet.ru/opennews/art.shtml?num=38971)
- [OpenSSH vs SSH](https://habr.com/ru/companies/ruvds/articles/751756/)
- [ASecuritySite: When Bob Met Alice](https://medium.com/asecuritysite-when-bob-met-alice/so-what-is-ed25519-and-what-does-it-have-to-do-with-curve-25519-f229804f2d08)
