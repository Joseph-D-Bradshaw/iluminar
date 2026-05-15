# Scroll Section — Always Fits the Viewport

The Artists section (`src/lib/components/Artists.svelte`) always fills the screen
on any device without scrolling within the section itself. This is how it works:

## The Pattern (5 rules)

```
section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 0;
  position: relative;
  overflow: hidden;
}
```

1. **`min-height: 100vh`** — never shorter than the viewport.

2. **`padding: 0`** — no vertical padding. Every pixel inside the section
   belongs to content, not spacing. Use spacing inside child elements instead.

3. **`display: flex; align-items: center`** — content floats in the middle.
   No `justify-content` or `flex-direction` overrides unless needed.

4. **Size items relative to the viewport** — use `vh` units clamped to a max:

   ```css
   .item {
   	width: min(40vh, 400px); /* shrinks on small screens, caps on large */
   }
   ```

   At a tighter breakpoint, reduce the `vh` multiplier:

   ```css
   @media (max-width: 640px) {
   	.item {
   		width: 30vh;
   	}
   }
   ```

5. **Absolutely position fixed UI** — titles, progress bars, labels that
   shouldn't flex or occupy space:
   ```css
   .section-title {
   	position: absolute;
   	bottom: 3rem;
   	left: 2rem;
   	z-index: 1;
   }
   ```

## Why it works

- `min-height: 100vh` guarantees the section is always ≥ the screen.
- `padding: 0` + `align-items: center` keeps the center of gravity in the
  middle regardless of screen height.
- Content sized in `vh` scales with the viewport — items occupy the same
  proportion of the screen on a 600px-tall phone as on a 1080px monitor.
- Absolute-positioned overlays don't participate in the flex layout, so they
  never push content around or create overflow.

## Checklist to replicate

- [ ] `min-height: 100vh`
- [ ] `padding: 0` (move padding to children)
- [ ] `display: flex; align-items: center`
- [ ] Key child dimensions use `vh` (clamped with `min()`)
- [ ] Fixed-position UI elements (title, progress) are `position: absolute`
- [ ] Responsive breakpoint reduces `vh` multiplier for small screens
