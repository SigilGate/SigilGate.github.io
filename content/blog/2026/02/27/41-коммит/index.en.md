---
title: "41 Commits"
slug: "41-commits"
date: 2026-02-27T00:00:00+00:00
draft: false
summary: "Today was a cleanup day. No new features, no forward movement on the map. But now several things work the way they were supposed to from the start."
categories: ["Project Diary"]
rubrics: ["Project Chronicles"]
tags: ["automation", "infrastructure", "scripts", "refactoring"]
---

Today was a cleanup day. No new features, no forward movement on the map. But now several things work the way they were supposed to from the start.

The first half of the day — legacy. One of the key scripts had lived in the monorepo since the earliest days of the project: ran as root, had paths hardcoded into the source, no configuration whatsoever. It worked — and that was enough. Now that we have a service user, environment variables, and unified scripting conventions, this piece looked like an old padlock on a new door. We straightened it out: rewrote it by the book, moved it to the `scripts` repository, updated the documentation — both in the repo and on the site. One block of functionality migrated and closed.

The second half — automating what was done manually or not done at all. Two repositories on Core live at different rhythms: the data repository is written here and needs to go to GitHub, the scripts repository evolves on GitHub and needs to come back here. Now that happens on its own, every 15 minutes.

The first test run of the new sync service — and the logs immediately read: "41 commits pushed." Exactly what we expected. All the changes in the data store had been accumulating on Core — that's not an architectural bug, it's the architecture's point. Scripts write, `commit.sh` commits. But until today that's where it ended: commits stayed on the node and went nowhere.

Seems simple enough. But the moment you start — questions appear. What if, while the script was sleeping, someone managed to make changes on both sides? In theory that shouldn't happen — but "in theory shouldn't happen" in infrastructure work sounds like an invitation to trouble. Had to think through conflicts, divergence, resolution strategy. In the end we chose a pragmatic compromise: Core always wins, and traces of any conflict remain in the repository — so nothing goes missing silently. Not perfect, but good enough for a working version.

---

I've [written before]({{< ref "blog/2026/02/22/как-развивать-проект-не-накапливая-технический-долг" >}}) about the kind of work that's invisible from the outside. It doesn't bring anything new — it only removes what quietly piled up or wasn't done properly. These days happen. They're necessary.

41 commits went to GitHub in the first seconds. A good result for a cleanup day.
