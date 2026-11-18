# Skills---
name: apple-design
description: Apple's approach to interface design and fluid, physical motion, translated for the web. Use for gesture-driven UI, springs, sheets, materials, typography, dark mode, reduced-motion.
---

# Apple Design

## Initial Response

When this skill is first invoked without a specific question, respond only with:

> I'm ready to help you build fluid, Apple-style interfaces on the web, my knowledge comes from Apple's WWDC design talks, translated for the web.

Do not provide any other information until the user asks a question.

How Apple builds interfaces that stop feeling like a computer and start feeling like an extension of you. This knowledge comes from Apple's WWDC design talks — chiefly *Designing Fluid Interfaces* (WWDC 2018) — distilled and translated into the web platform (CSS, Pointer Events, `requestAnimationFrame`, spring libraries like Motion/Framer Motion).

The through-line: **an interface feels alive when motion starts from the current on-screen value, inherits the user's velocity, projects momentum forward, and can be grabbed and reversed at any instant.** Springs are the tool that makes all of this natural, because they are inherently interruptible and velocity-aware.

## The Core Idea

An interface is fluid when it behaves like the physical world: things respond instantly, move continuously, carry momentum, resist at boundaries, and can be redirected mid-motion.

Apple frames design as serving four human needs: **safety/predictability, understanding, achievement, and joy.** Every rule here serves one of them.

## 1. Response — kill latency

- **Respond on pointer-down, not on release.** Highlight a button the instant it's pressed.
- **Be vigilant about every latency.** Audit debounces, artificial timers, transition waits, and the ~300ms tap delay.
- **Feedback must be continuous during the interaction.** For a drag, slider, or drawer, update the UI 1:1 with the pointer the whole way through.

```css
.button:active {
  transform: scale(0.97);
  transition: transform 100ms ease-out;
}
```

## 2. Dark Mode

Dark mode is a full appearance, not an inverted palette.

- **Honor the system setting.** Default to `prefers-color-scheme`, declare `color-scheme: light dark`, and offer an override only as a convenience.
- **Use semantic tokens, never raw colors.** Define roles (background, elevated background, label, secondary label, separator, tint) and swap their values per appearance.
- **Elevation gets lighter, not shadowed.** Raised surfaces (sheets, popovers, cards) step up: #1C1C1E, #2C2C2E. Shadows barely read in dark mode.
- **Dim secondary text** to cut glare; reserve full white for primary labels.
- **Brighten accent colors** (e.g. #007AFF → #0A84FF) so tints keep contrast on dark backgrounds.
- **Materials stay translucent.** Use `backdrop-filter: blur() saturate()` with a dark-tinted fill.
- **Check contrast in both appearances** (WCAG AA, 4.5:1 for body text).
- **Images and icons:** provide dark variants via `<picture>` with `media="(prefers-color-scheme: dark)"`.

```css
:root {
  color-scheme: light dark;
  --bg: #FFFFFF;
  --bg-elevated: #F2F2F7;
  --label: #000000;
  --label-secondary: rgba(60,60,67,0.6);
  --separator: rgba(60,60,67,0.29);
  --tint: #007AFF;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #000000;
    --bg-elevated: #1C1C1E;
    --label: #FFFFFF;
    --label-secondary: rgba(235,235,245,0.6);
    --separator: rgba(84,84,88,0.6);
    --tint: #0A84FF;
  }
}
.material {
  background: color-mix(in srgb, var(--bg-elevated) 72%, transparent);
  backdrop-filter: blur(20px) saturate(180%);
}
```

- **Switching appearances** should be instant or a short cross-fade (≤200ms); never animate every color individually.
