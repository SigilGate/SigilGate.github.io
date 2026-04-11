---
title: "New Device!"
slug: "new-device"
date: 2026-02-15T18:00:00+00:00
draft: false
summary: "The ice has broken: an automation scripts repository is up, the architecture is in place, and the first scenario — adding client devices — is already working."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["automation", "scripts", "infrastructure"]
---

I know I promised to do this last weekend. But back then I was blocked by the lack of a data storage system — which made it impossible to build proper automation. How do you work with data when there's practically no data? Unsystematic data is just garbage. Information noise, nothing more.

Last weekend I [developed a data storage concept]({{< ref "blog/2026/02/14/data-storage-system" >}}) — and during the week implemented it as a file repository in a private git repo. Time to put it to work! I hereby declare the automation process open!

## The Ice Has Broken

I created the [scripts](https://github.com/SigilGate/scripts) repository — and now all previously written scripts will gradually migrate there. Everything related to initial setup, deployment, security configuration, and other routine operations.

## Architecture and Security

I developed a general architecture for the scripts — and perhaps the most interesting question turned out to be not "how to write a script," but **"how to make the repository public without shooting yourself in the foot."**

The scripts repository is open. This is a matter of principle — transparency and open source are our credo. But the data it works with lives in a private storage. The scripts say nothing about the storage, except that it exists. We pass storage information through an environment variable — writing the path to the data repository into it. We do the same with all other secrets — passwords, SSH keys, addresses — they live in environment variables on the Core node.

Now scripts receive sensitive data through parameters — which means they can safely be published as open source! Not a single hardcoded value in the code. This was one of the key requirements, and the old scripts, written in the spirit of "Let's just get this done quickly!", frankly violated it.

The second important decision — **splitting scripts into two types**. Atomic scripts do exactly one thing: create a record, add a client to a node, commit changes. Orchestrators chain them into sequences for specific scenarios. This gives readability, reusability, and — what's especially valuable — a clear migration path to Python in the future. Atomic scripts will become functions, orchestrators will become CLI commands. The foundation is already laid.

## First Run: New Devices

And the first thing I decided to test this on — adding new client devices. Long-awaited.

Adding them manually is always painful: you need to change data in several places, and the risk of error — one misplaced comma or space in the configs — can bring down the entire chain. Something needed to be done about this — and now we have everything we need.

One script call:

```bash
./devices/add.sh --user 2 --device "ios_3"
```

The script finds the user in the storage, generates a UUID, determines which Entry nodes serve this user, adds the client to each node via SSH, restarts the service — and outputs a ready-made VLESS link. What used to require manually gathering parameters from five different files and carefully editing JSON configs.

The first test on production infrastructure crashed, of course. The script cheerfully reported "client added, Xray active" — but the client couldn't connect. Turned out that heredoc scripts weren't reaching the remote node: the sudo password and the script body were competing for the same stdin. A classic of remote execution via SSH — everything seems right, but the input streams got tangled up. Fixed it, double-checked — and after that everything went smoothly. Five devices added with five calls, five atomic commits in the storage — a clean, transparent change history.

## What's Next

We've laid the foundation for a user device management system and set the groundwork for automating all other processes. Next — we'll build new components on top of it:

- Device deletion and deactivation.
- Storage synchronization between nodes — push on a timer with collision resolution.
- Node preparation — from initial setup to service deployment.

And much more — I've got grand plans!
