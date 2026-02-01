---
title: "First impressions of the new theme"
date: 2026-02-01T04:00:00+00:00
draft: false
summary: "Congo works great out of the box, but one small detail needed a manual fix."
tags: ["site", "hugo", "theme"]
---

After switching to Congo, the first thing you notice is that everything just works. Dark mode, language switching, navigation — nothing broke, and it all looks clean and modern.

## One small detail

There's something that has always puzzled me: why do the vast majority of themes default to left-aligned text? The right edge of paragraphs ends up ragged, and it looks sloppy — especially on wide screens where the lines are long and the unevenness is hard to miss.

Justified text makes everything feel tighter and more intentional — like a book or a well-typeset magazine. Sure, justify has its quirks with hyphenation, but for a blog with reasonable line lengths it looks significantly better.

## How we fixed it

Congo provides an `assets/css/custom.css` file for user styles — it's loaded automatically and isn't touched during theme updates. A single rule is all it takes:

```css
.prose {
  text-align: justify;
}
```

Done. Text is now justified, and the next Congo update won't break anything.
