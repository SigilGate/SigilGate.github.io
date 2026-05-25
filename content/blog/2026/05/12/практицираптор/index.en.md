---
title: "Terminal Coding Trainer"
slug: "practiciraptor"
date: 2026-05-15T00:00:00+00:00
draft: false
summary: "Revived an old pet project of a learning platform and built a minimalist coding trainer — with a container pool for each language and vim as the IDE."
categories: ["Project Diary"]
rubrics: ["Margin Notes"]
tags: ["practicaraptor"]
---

Since we're already on the subject of side projects — I have to mention one more that I recently polished up.

I have a long-running pet project that seemed dead but occasionally resurfaces, and my hands start itching to add something to it. I'm talking about a learning platform project.

One of the repositories from that project I recently decided to pull out and finish up. It's a backend for running educational programming assignments: a widget embeddable in static HTML pages that connects to a FastAPI backend and delivers a terminal with a ready-to-go environment for the task. Within the learning platform, I planned to use it to embed hands-on coding exercises directly into theoretical courses.

I had almost completely forgotten I had such a wonderful project. I got fairly far with it — managed to implement a container with the environment and a web-based terminal that served it on the frontend via Flask. But that's where it stopped: VPN access issues came up, my thoughts drifted elsewhere. I never got to finish what I'd originally planned — pre-configured environments for different languages and practical assignments for each. Something like LeetCode or CodeWars, but built on terminal-based solutions.

From this same project, by the way, grew [ai-box]({{< ref "blog/2026/05/12/ai-box" >}}), which I covered last time.

Anyway, I decided to come back to it and shake off the dust. Several reasons led me here.

Constraints are closing in from all sides. The internet is shrinking — unevenly, but steadily; it's reached GitHub now, and that's getting uncomfortably close. On the other side — one of my key AI providers' subscriptions disappeared from our market: I managed to snag a yearly plan in the last month it was still available. There are worrying rumors about tightening restrictions — maybe it's temporary, pre-IPO jitters, or maybe it's a trend and things will only get worse. Who knows.

Either way, riding this wave I decided to get back to writing code by hand — sat down with LeetCode, started learning Go. And then I thought: why not dust off this project? It's basically a ready-made coding trainer with not much left to finish.

That's how **[Practiciraptor](https://practiciraptor.com)** was born — a task trainer for quick coding practice. The name comes from **Velociraptor** — the fastest and most aggressive creature from the prehistoric era.

As everyone knows (in programming and beyond), the best theory is practice. Learning new languages and frameworks works better through your hands than your head. For small code snippets, quick experiments, checking how a particular expression behaves — you often just want to open a terminal and throw in some code. See the output, experiment, close it and forget it — without all the foreplay (setting up an environment, installing tools, creating a project structure). This isn't about working on a big project and saving results — this thing is built for quick, disposable sessions with total cleanup afterward. Say hello!

FastAPI backend. Deliberately kept simple: I decided to skip all the complex logic, run buttons, validation, and everything else you usually find in these trainers. Honestly — I went full geek on this one. I say that with a satisfied smirk.

For each language, a pool of three containers stands ready to launch on demand. Each container has a terminal with a configured working environment (compiler or interpreter, helper libraries, all that stuff), plus a text editor. And of course that's vim — as the IDE for writing code. Full geek, I told you.

Assignments are stored directly in the container image and launch from the browser already open at the right task number. Right now, the base set has three assignment types: write an expression, implement a function (for algorithmic problems), or modular — when code across multiple files is needed. An assignment includes a description (printed to the terminal on startup), project structure (files to work with), and tests. After the session ends, the container is destroyed, and the next one from the pool spins up — this way you can support up to three simultaneous sessions.

For starters — three languages: Bash, Python, Go. Rust didn't make the cut: the distribution turned out too heavy for my half-dead, nearly-full virtual machine. But the other three — just fine.

Go isn't chosen by chance: I've been eyeing this language lately, and it feels like we're on the same wavelength.

This little thing probably won't become commercial or popular, but as a working tool for grinding algorithms and quick practical exercises — it does the job. I think the current implementation can even survive a surge in users (of up to 3 people))).

You can check out the demo at [practiciraptor.com](https://practiciraptor.com). Fair warning: under the hood there's a JS framework being served via CDN, and without a VPN it probably won't fly. The sixteen-kilobyte curse caught up with me here too.
