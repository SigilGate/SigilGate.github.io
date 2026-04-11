---
title: "Data Storage System"
slug: "data-storage-system"
date: 2026-02-14T03:00:00+00:00
draft: false
summary: "The technical side of the project hasn't been standing still — I structured the data, chose storage formats, and laid the groundwork for future automation."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["new", "architecture", "data", "automation"]
---

Reading the recent posts, you might get the impression that all I've been doing is working on the blog and reading the news, while the technical side of the project stood still.

~~Yes, that's true!~~

In reality — I was indulging in... contemplation. Yes, I didn't do much. But that little bit is a very important part of a huge body of work ahead.

## Data Storage

I was preparing the data storage system: structuring account and technical data, creating a repository for it, and converting it from text to digital format — building a foundation for future automation. The structures I've created will become the basis for data models when I finally commit to automating everything and start rewriting the scattered bash scripts (which I currently use to automate routine operations) into a unified orchestrator application in Python.

## Why Do We Need etcd?

Remember the recent [article about KV storage]({{< ref "blog/2026/02/07/data-storage" >}})? An attentive reader might have had a question when I wrote about etcd: to deploy a cluster, we need at least three core nodes — but at the prototype stage, we're working with just one.

The truth is — a KV store on etcd is overkill at the prototype stage. We'll get to using it only at the growth stage. However, by designing the architecture from the start and thinking about it now, we avoid a multitude of problems with reworking and rebuilding things in the future.

At the current stage, a git repository is perfectly sufficient for static data storage. By setting up periodic auto-commits, we can back up configurations — and if needed, track the dynamics of changes over time. Rare and minor changes of small volume — at the prototype stage, this kind of data storage system is more than enough.

But looking ahead — even now it's worth identifying the core entities and defining them as KV structures for future storage in etcd. Choosing storage formats — TOML, YAML, JSON? I went with JSON, by the way — it's easier to parse. And besides, "shuffling JSONs around" is a classic.

## What's Done

So that's exactly what I was doing. I pulled apart all the data that lived in descriptive text files — account credentials, server parameters, paths, addresses, node types, and so on — into a structured private repository with JSON files. Now all the data is saved, everything is clean, structured, ready to go into a database at a moment's notice — and the original monorepo is one folder lighter.

## What's Next

Next on the agenda — creating a private repository where I'll move (and in some cases rewrite) all the existing bash scripts for deploying and configuring nodes, as well as creating scripts for working with account data — user management. The data is already there — time to start working with it.

And after that — rewriting the scattered bash scripts into a unified orchestrator application in Python. But I'll think about that ~~tomorrow~~ the day after tomorrow.
