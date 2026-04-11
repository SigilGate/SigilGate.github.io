---
title: "5. Core Content Rubrics"
description: "Rubric structure of Sigil Gate content"
summary: "Two tracks, four rubrics, nine sub-rubrics"
weight: 50
---

{{< lead >}}
**Sigil Gate** content is organized into a three-level structure: track — rubric — sub-rubric.
{{< /lead >}}

- **Track** — a thematic direction (what we write about).
- **Rubric** — a substantive dividing line, tied to an audience segment.
- **Sub-rubric** — a thematic tag for navigation and promotion. Boundaries between sub-rubrics are fluid: a single piece can belong to multiple sub-rubrics at once.

The real dividing line runs along the first and second levels. The third level is a marketing layer.

---

## 1. Technical Track

### Rubric 1: Architecture and Research

**Primary audience:** Specialist.

The backbone of the blog. Deep technical content: network internals, protocols, threat models, architectural decisions, technology choices, tool analysis. The style varies from strictly technical to popular science — depending on the topic and the audience of a particular piece.

**Platforms:** website, Habr, Medium.

#### "Under the Hood"

Topology, principles, architectural decisions. How the network works from the inside: nodes, links, flows, routing logic. Content for those who want to understand the design, not just the result.

- *Blog examples:* data-storage, kv-cluster, network-principles.

#### "Protective Runes"

Masking protocols, traffic obfuscation, cryptography. How the network defends itself against detection and analysis: TLS, VLESS, gRPC, statistical invisibility, threat model.

- *Blog examples:* path-rotation, statistical-invisibility, when-targeting-hits-the-mark.

#### "Crossroads"

Technology choices, comparison of approaches, ADRs. Points where a direction must be chosen: why etcd and not Consul; why systemd timer and not cron; why gRPC and not WebSocket. Well-reasoned decisions with analysis of alternatives.

- *Blog examples:* kv-cluster, systemd-timer.

---

### Rubric 2: Project Diary

**Primary audience:** User and Client.

Build in public. What's happening with the project, what's been done, where we're heading. A living project with a human face — but the backbone is always technical: product and infrastructure development. Content in this rubric builds trust: the reader sees that real people with real experience stand behind the project.

**Platforms:** website, Telegram.

#### "Margin Notes"

Day-to-day observations, discoveries, small finds, WIP. Everything that doesn't warrant a full article on its own but is worth sharing.

- *Blog examples:* first-impressions, systemd-timer, switching-to-congo.

#### "Project Chronicles"

Significant milestones, important changes, turning points. A new section on the website, a new architectural capability, a shift in approach — events that change the scale or direction of the project.

- *Blog examples:* about-rewrite, docs.

---

## 2. Philosophical Track

### Rubric 3: Technology and Society

**Primary audience:** Thoughtful reader.

Reflection at the intersection of technology, politics, and philosophy. This content is broader than **Sigil Gate**: it addresses questions that concern everyone — freedom of information, the impact of technology on society, lessons from the past. Popular science style with an editorial perspective.

**Platforms:** website, Zen, Medium.

#### "Digital Borders"

Freedom of information, censorship, regulation, blocking. How states control the internet, how it affects people, and what can be done about it.

- *Blog examples:* statistical-invisibility.

#### "Horizon Line"

The future of technology: AI, neural networks, new protocols, the evolution of the internet. Where the world is heading and what it means for privacy, freedom, and human connection.

- *Examples:* (planned).

#### "Invisible Threads"

The not-always-obvious connection between ideas of the past and the current stage of technological development. Bakunin and federated networks, Kropotkin and mutual aid in P2P, steganography and TLS — threads that link eras.

- *Blog examples:* network-principles (Bakunin, Kropotkin), network-federation.

---

### Rubric 4: Editorial Perspective

**Primary audience:** Thoughtful reader + User and Client.

Personal territory. Who stands behind the project, where they came from, what they believe in. The concrete edge of the philosophical track: not abstract ideas, but a person with their experience, principles, and stance. Content in this rubric creates an emotional connection and builds trust on a human level.

**Platforms:** website, Zen.

#### "Starting Point"

Biographical essays, the path to the project, key moments in life and career. Where it all began and how it led here.

- *Examples:* (planned).

#### "Manifesto"

Principles, values, professional credo. A public declaration of what we believe in and why the project exists.

- *Examples:* (planned).

#### "Thinking Aloud"

Personal stance on specific events and questions. Reactions, reflections, musings — without claiming to hold the truth, but with the right to one's own voice.

- *Examples:* (planned).
