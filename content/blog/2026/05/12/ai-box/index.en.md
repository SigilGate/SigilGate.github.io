---
title: "AI in the Browser"
slug: "ai-box"
date: 2026-05-12T00:00:00+00:00
draft: false
summary: "One more reason for the silence: while reorganizing my workspace, I built a small project — a browser terminal for an AI assistant, accessible from any device."
categories: ["Project Diary"]
rubrics: ["Margin Notes"]
tags: ["ai-box"]
---

The period of relative quiet on the blog had one more reason I didn't mention last time.

I was reorganizing my workspace — which meant I sometimes had no access to AI assistants. My workstation moved, and I've been at the computer less often. At some point I thought it over and decided to fix the situation by looking at it from a slightly different angle: bring the AI assistant into the browser — so it would be accessible from anywhere, including mobile devices.

It distracted me from the main project a bit — but gave my brain a break, a reason to switch gears and puzzle over things that aren't always obvious: how to combine several disparate components into one system.

That's how **[ai-box](https://github.com/A1eksMa/ai-box)** came to be.

The project is not very large — just about five hundred lines. But its complexity lies elsewhere. The stack works like this: a browser terminal built on **xterm.js** connects via WebSocket to a **FastAPI** backend, which communicates with a **tmux** session through PTY. Inside tmux lives the AI assistant. All of this is packed into a **Docker** container; outside — **nginx** with SSL. A volume on the host stores authorization data and working files — restarting the container wipes nothing important.

The key piece is tmux. It keeps the session alive as long as the container is running. The connection drops, the browser closes — tmux doesn't notice. When you reconnect, the browser simply attaches to the already-live session.

I lost some time on this — but now I have AI access from anywhere, on any device. And most importantly: the connection doesn't break, there's no need to restore context — you can pick up right where you left off last time.

I'm typing this post in it right now. Checking how it works...
