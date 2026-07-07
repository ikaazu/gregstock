# gregstock.com — Design Fixes Handoff

**Files to edit:** `assets/css/style.css` (primary), `index.html` (secondary — content change only) **Context:** Plain HTML + CSS site, no build framework, no Tailwind. All fixes are in `style.css` unless noted. The color token system, dark/light theme switcher, layout structure, spacing rhythm, responsive breakpoints, and copy are all solid — do not touch them. These fixes are targeted at five specific patterns flagged by the vibecoded-design-tells catalog.

Work through the fixes in order. Each one is independent — if one needs a judgment call, skip it and move to the next.

------

## Fix 1 — Remove gradient text from the hero H1 (HIGH PRIORITY)

### The problem

The hero heading applies `.gradient-text`, which clips a three-stop gradient through the text:

```css
.gradient-text {
  background: linear-gradient(135deg, var(--text), var(--accent) 48%, var(--accent-2));
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
```

Gradient-filled heading text is Tell 3 in the catalog — the highest-upvoted specific complaint in the dataset. It reads as generated even when the colors themselves are intentional.

### The fix

Replace the gradient clip with a solid accent color. Keep the class name in case it's used elsewhere, but change what it does:

**Before:**

```css
.gradient-text {
  background: linear-gradient(135deg, var(--text), var(--accent) 48%, var(--accent-2));
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
```

**After:**

```css
.gradient-text {
  color: var(--accent);
}
```

This makes the name render in the light blue accent color — visible, on-brand, and clearly a made choice rather than a default effect. No HTML changes needed.

------

## Fix 2 — Reduce body background gradient opacity (HIGH PRIORITY)

### The problem

The body has three stacked gradients baked in as atmosphere:

```css
body {
  background:
    radial-gradient(circle at top left, rgba(var(--accent-rgb), 0.24), transparent 34rem),
    radial-gradient(circle at 80% 10%, rgba(var(--accent-2-rgb), 0.18), transparent 28rem),
    linear-gradient(135deg, var(--bg) 0%, var(--bg-soft) 48%, var(--bg) 100%);
}
```

The 0.24 opacity blue spot at top-left is very visible and reads as a decorative blob gradient — one of the catalog's Tell 3 compound signals. The amber spot at 80% 10% reinforces it. Combined with the gradient heading, gradient button, gradient brand mark, and gradient avatar, this is gradient saturation.

### The fix

Pull the radial blob opacities down significantly so they read as depth rather than decoration, and remove the `linear-gradient` base layer — the solid `--bg` color handles the base on its own:

**Before:**

```css
body {
  margin: 0;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  color: var(--text);
  background:
    radial-gradient(circle at top left, rgba(var(--accent-rgb), 0.24), transparent 34rem),
    radial-gradient(circle at 80% 10%, rgba(var(--accent-2-rgb), 0.18), transparent 28rem),
    linear-gradient(135deg, var(--bg) 0%, var(--bg-soft) 48%, var(--bg) 100%);
  min-height: 100vh;
  line-height: 1.6;
}
```

**After:**

```css
body {
  margin: 0;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  color: var(--text);
  background:
    radial-gradient(circle at top left, rgba(var(--accent-rgb), 0.07), transparent 34rem),
    radial-gradient(circle at 80% 10%, rgba(var(--accent-2-rgb), 0.05), transparent 28rem),
    var(--bg);
  min-height: 100vh;
  line-height: 1.6;
}
```

The radial spots stay but become genuinely subtle — they add depth without announcing themselves.

### Also: remove the profile card blob

The `.profile-card::before` pseudo-element adds another radial blur blob:

```css
.profile-card::before {
  content: "";
  position: absolute;
  inset: -80px -80px auto auto;
  width: 180px;
  height: 180px;
  border-radius: 999px;
  background: rgba(var(--accent-rgb), 0.18);
  filter: blur(4px);
}
```

Remove this entirely. The card already has `box-shadow: var(--shadow)` for depth. The blob pseudo-element is redundant and contributes to gradient saturation.

**After (remove the entire rule):**

```css
/* .profile-card::before — removed */
```

------

## Fix 3 — Replace gradient on primary button with solid fill (HIGH PRIORITY)

### The problem

The primary button uses the same blue-to-amber gradient as the hero heading, brand mark, and avatar:

```css
.button.primary {
  color: #0e1117;
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  border: 0;
}
```

Having the gradient appear on heading text, button, logo mark, and avatar simultaneously makes every element feel like it was styled with a single token. Fixing the heading (Fix 1) helps; fixing the button removes the redundancy entirely.

### The fix

Solid accent fill on the button. It's already high-contrast against dark text (`color: #0e1117`), so it reads clearly without needing a gradient:

**Before:**

```css
.button.primary {
  color: #0e1117;
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  border: 0;
}
```

**After:**

```css
.button.primary {
  color: #0e1117;
  background: var(--accent);
  border: 0;
}
```

The brand mark and avatar can keep their gradient — those are visual elements, not interactive controls, and one gradient used consistently in identity elements is fine. The issue was the same gradient on text + button + mark + avatar simultaneously.

------

## Fix 4 — Replace Inter with a more distinctive font (HIGH PRIORITY)

### The problem

```css
font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
```

Inter is the catalog's Tell 8 — the "no decision was made" font for dark tech sites. On a personal portfolio with genuinely distinct copy, the font should match the personality.

### The fix

Choose one of the following and apply it. Each is a Google Font (add the `<link>` import to `<head>` in `index.html`) or a system font (no import needed):

**Option A — DM Sans (recommended)** Warmer and more approachable than Inter, still clean and highly legible, distinct without being loud. Good fit for a personal site that mixes tech and creative content.

```html
<!-- Add to <head> in index.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300..700;1,9..40,300..700&display=swap" rel="stylesheet">
/* In style.css, replace the font-family line in body: */
font-family: 'DM Sans', ui-sans-serif, system-ui, -apple-system, sans-serif;
```

**Option B — System font stack only (no import needed)** Leans into the native Mac/Windows feel. Clean, fast, zero-dependency, and reads as a deliberate minimalist choice rather than an overlooked default.

```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
```

**Option C — IBM Plex Sans (tech/editorial hybrid)** Has a slightly technical flavor that suits the IT + creative mix well. Distinctive at larger sizes.

```html
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300;400;600;700&display=swap" rel="stylesheet">
font-family: 'IBM Plex Sans', ui-sans-serif, system-ui, -apple-system, sans-serif;
```

Pick one. If in doubt, go Option A (DM Sans). Do not mix fonts — one family site-wide.

------

## Fix 5 — Reduce card border-radius and break the pill-everything pattern (HIGH PRIORITY + MEDIUM)

### The problem

Two related issues:

1. `--radius: 24px` is applied to all cards — large rounded corners that read as a Tailwind `rounded-2xl` default even when handwritten.
2. Every interactive element is `border-radius: 999px` (full pill): nav links, theme buttons, eyebrow badge, both hero buttons, contact cards. No shape variation anywhere.

### Fix 5a — Reduce the card radius token

**Before:**

```css
:root {
  /* ... */
  --radius: 24px;
}
```

**After:**

```css
:root {
  /* ... */
  --radius: 10px;
}
```

This affects `.card` and `.profile-card` automatically wherever `var(--radius)` is used. The `.stat` block uses `border-radius: 18px` independently — update that to `8px` while you're here:

```css
/* Before */
.stat {
  padding: 14px;
  border-radius: 18px;
  background: var(--stat-bg);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

/* After */
.stat {
  padding: 14px;
  border-radius: 8px;
  background: var(--stat-bg);
  border: 1px solid rgba(255, 255, 255, 0.08);
}
```

The avatar (`border-radius: 30px`) and brand mark (`border-radius: 14px`) can stay — those are portrait/logo elements where heavy rounding is appropriate and distinct from card rounding.

### Fix 5b — Give the hero buttons a real radius instead of full pill

The primary and secondary hero buttons don't need to be pills. The nav links and theme controls can stay as pills (they're small utility controls where pills are appropriate). But the main action buttons should have a distinct shape:

**Before:**

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 46px;
  padding: 0 18px;
  border-radius: 999px;
  border: 1px solid var(--card-border);
  font-weight: 700;
  transition: 160ms ease;
}
```

**After:**

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 46px;
  padding: 0 20px;
  border-radius: 10px;
  border: 1px solid var(--card-border);
  font-weight: 700;
  transition: 160ms ease;
}
```

This matches the new `--radius` value and creates a distinction between "utility pill controls" (nav, theme toggles) and "primary action buttons" — which is a real design decision.

------

## Fix 6 — Remove Argent Ledger placeholder from Projects section (MEDIUM — content change)

### The problem

In `index.html`, the Projects section contains a placeholder entry:

```html
02 — Argent Ledger / More details coming soon.
```

A numbered slot with no content creates dead visual weight and tells the visitor nothing about the project. It reads as scaffolding that wasn't cleaned up.

### The fix

**Option A (recommended):** Remove the entry entirely until the project has something to say. Two real project cards are better than two real cards plus a placeholder.

**Option B:** Replace the placeholder text with a one-sentence description of what Argent Ledger actually is, even if it's early-stage. Something like:

```
02 — Argent Ledger
A personal finance tracking tool for people who hate spreadsheets. Currently in early development.
```

Either option is better than "More details coming soon."

------

## What NOT to change

- **Color token system** — `--bg`, `--accent`, `--accent-2`, `--muted`, `--card`, `--card-border`, and all variants. Custom and deliberate throughout.
- **Dark/light theme switcher** — The JavaScript and `html[data-theme="light"]` overrides. Leave entirely untouched.
- **Spacing system** — `44px 0` section padding, `22px` card padding, `18px` grid gap, `14px` hero action gap. Consistent and well-applied throughout.
- **Responsive breakpoints** — The 860px and 560px media queries. Both are well-placed and collapse gracefully.
- **Brand mark and avatar gradients** — These can keep `linear-gradient(135deg, var(--accent), var(--accent-2))`. The issue was the same gradient on heading text and buttons simultaneously, not the identity elements.
- **Nav pill links and theme control pills** — These can stay as `border-radius: 999px`. Small utility controls are fine as pills; the distinction being created is between utility controls (pills) and primary action buttons (10px radius).
- **`160ms ease` transitions** — Functional, minimal, keep as-is.
- **The pulse dot** — The `box-shadow: 0 0 0 6px` ring on `.pulse` is a functional status indicator, not decorative glow. Leave it.
- **All copy** — The hero tagline, About blurb, Now section, project descriptions, and footer text. Clean, distinct, and should not be touched.
- **The hero two-column grid layout** — The `grid-template-columns: minmax(0, 1.15fr) minmax(280px, 0.85fr)` is better than a centered hero. Keep it.

------

## Verification checklist

After making changes, confirm in both dark and light modes:

- [ ] Hero H1 renders in solid `var(--accent)` blue — no gradient clip visible
- [ ] Body background blobs are barely perceptible (subtle depth, not visible spots)
- [ ] No `.profile-card::before` blob visible behind the profile card
- [ ] Primary button is solid light blue — no gradient fill
- [ ] Font has changed from Inter — confirm in DevTools (Elements → Computed → font-family on body)
- [ ] Cards have noticeably smaller corner radius (10px vs 24px)
- [ ] Stat blocks have smaller corner radius (8px vs 18px)
- [ ] Hero buttons are rectangular-ish (10px) while nav links remain pill-shaped (999px)
- [ ] Argent Ledger placeholder is removed or replaced with real copy
- [ ] Light mode still functions correctly via theme switcher
- [ ] No layout shifts, overflow issues, or broken responsive behavior at 860px and 560px