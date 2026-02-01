---
title: "First impressions of the new theme"
date: 2026-02-01T04:00:00+00:00
draft: false
summary: "Congo works great out of the box, but..."
tags: ["site", "hugo", "theme"]
---

After switching to Congo, the first thing you notice is that everything just works. Dark mode, language switching, navigation — nothing broke, and it all looks clean and stylish.

## One small detail

There's something that has always puzzled me: why do the vast majority of themes default to left-aligned text? Why? Just... why?! Oh, for the love of — my inner perfectionist suffers every time I see a crisp edge on the left and a ragged, uneven zigzag of line endings on the right. What's stopping us from setting proper rules and boundaries for text blocks? It looks really sloppy — especially on wide screens where the lines are long and the unevenness is hard to miss.

Justified text makes everything feel tighter and more intentional — like a book or a well-typeset magazine. Sure, justify has its quirks with hyphenation, but... justified alignment is undoubtedly my choice.

## How to fix it

Congo provides an `assets/css/custom.css` file for user styles — it's loaded automatically and isn't touched during theme updates. A single rule is all it takes:

```css
.prose {
  text-align: justify;
}
```

Done. Text is now justified, and the next Congo update won't break anything.
