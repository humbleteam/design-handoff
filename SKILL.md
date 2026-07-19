---
name: design-handoff
description: Turns a mockup, screenshot, or HTML file into a dev-ready handoff spec - layout measurements, a token map, a component-state inventory, accessibility notes, and numbered open questions. Use for "write a handoff spec for this screen", "turn this mockup into dev docs", "document component states", or "what tokens does this screen use". Not for critiquing design quality or generating new UI - see design-review or html-mockup.
---

# Design handoff

Turn a finished mockup into a spec a developer can implement without asking you a follow-up question for anything that was actually visible in the source - and without you inventing an answer for anything that wasn't.

## Input

Accept any of:
- One or more mockup images or screenshots, pasted into chat or given as file paths.
- An HTML file or a URL to a live page.
- An optional token set: a CSS file with custom properties, a JSON token file, or a plain description of existing design tokens (palette, type scale, spacing ramp, radii, shadows).

If no token set is given, say so in the output and derive a draft one from the mockup itself (see Step 3).

## Output

One markdown file per screen, named `<screen-slug>-handoff.md`, following the eight-section structure in [`references/handoff-template.md`](references/handoff-template.md). Read that file for the exact skeleton to copy before writing the first spec.

**Decide screen count first.** If two or more inputs show the same layout with only a component's state changed (a button in default and hover, a form before and after an error), that is ONE screen - merge the evidence into section 4 of a single spec. If the inputs show different screens, routes, or breakpoints, write one spec file per screen. For a screen supplied at multiple breakpoints, write one file per screen and add a subsection per breakpoint under section 2 (Layout) rather than duplicating the whole file.

## The spine: never invent what you can't see

This is the rule every other step serves. If a value, state, or behavior is not directly visible in the source material:
- Do not guess it.
- Do not derive it by "reasonable default" (no darkening a color 10% for a hover guess, no assuming a 200ms fade because that's common).
- Write `OPEN QUESTION` and add it to section 8, numbered, tagged `BLOCKING` or `NON-BLOCKING`.

A spec with ten open questions because the source only shows one state is doing its job. A spec with zero open questions because you filled every gap with a plausible guess has failed at the one thing it exists to do.

## Steps

### 1. Screen summary

Record dimensions (from image size, HTML viewport, or a stated breakpoint - mark `(estimated)` if inferred from proportions rather than measured), a one-sentence purpose, and how many source images or files fed this spec.

### 2. Layout

Walk the screen in reading order (top to bottom, left to right within a row) and list every region: name, position or size, internal padding, and the gap to the next region. Use whatever units the source supports - exact pixels from HTML/CSS, or `(estimated)` proportions from a static image. If two sources of the same screen show conflicting spacing (e.g., two screenshots at slightly different padding), flag the conflict explicitly in this section. Do not average the two values and do not silently pick one.

### 3. Token map

List every distinct color, font, spacing value, corner radius, and shadow that appears on the screen, exactly once each, with four columns: property, observed value, mapped token, status.

- If a token set was provided, match each observed value to the nearest existing token. Use a small tolerance (a few px for spacing, a close visual neighbor for color) - if nothing matches within that tolerance, the value is new.
- Values with no match get `NEW-TOKEN: <proposed-name>` in the mapped-token column, following the naming convention of the existing tokens if one is visible (e.g., `color-<role>`, `space-<n>`, `radius-<size>`), or a plain role-based kebab-case name if there is no existing convention to follow.
- If no token set was provided at all, every row is `NEW-TOKEN` - draft a full proposed set, and say plainly in the spec that none of it is confirmed against a real system yet.

### 4. Component inventory

For every distinct interactive element (buttons, inputs, cards, nav items, toggles, and so on), enumerate exactly these eight canonical states: default, hover, focus, active, disabled, loading, empty, error.

For each state, mark one of:
- A concrete spec line (fill, label, border, icon), only if that state is actually visible in the source.
- `not applicable` with a one-clause reason (a static label has no hover state; a single-item list has no empty state).
- `OPEN QUESTION #n` if the state is plausible for this component but isn't shown.

If the source shows only a default state for a whole screen, say that plainly at the top of this section ("Source shows default state only - states table below is mostly open questions by design") rather than padding the table with invented hover and focus colors.

### 5. Behavior notes

Transitions, scroll behavior (sticky headers, infinite scroll, parallax), and validation timing (on blur, on submit, on keystroke) - but only what the source directly shows or states in an annotation. An arrow icon implies navigation; it does not imply a transition style. Mark anything speculative as an open question instead of describing it as fact.

### 6. Accessibility annotations

- Landmark roles inferred from visual structure (header, nav, main, list, button, dialog).
- Accessible names proposed from visible text or, for icon-only controls, from the icon's meaning.
- A proposed focus order matching reading order and visual hierarchy, numbered.
- Contrast ratios for every foreground/background text pairing that appears in the token map, checked against WCAG 2.2 SC 1.4.3 (4.5:1 for normal text, 3:1 for large text) and SC 1.4.11 for non-text UI components. If the exact hex can't be extracted from the source, say the ratio is estimated and flag it as an open question if it's close to the threshold.

### 7. Assets list

Every image, icon, or illustration that needs export: recommended format (SVG for icons and illustrations, PNG or WebP for photos), and size(s) - `@1x/@2x/@3x` for raster assets, single file for vector.

### 8. Open questions

Collect every `OPEN QUESTION` raised in sections 2 through 6 into one numbered list. Tag each:
- `BLOCKING` - implementation cannot proceed correctly without an answer.
- `NON-BLOCKING` - implementation can proceed with a stated reasonable default, revisit before ship.

Number continuously across the whole spec (the numbers referenced inline in sections 2-6 must match this list).

## Edge cases

- **Only one state shown for the whole screen.** Expected outcome, not a failure - the component inventory will read mostly `OPEN QUESTION`. Say so explicitly rather than inventing hover/focus/active to make the table look complete.
- **No token set provided.** Derive a full draft token set from the mockup and mark every single row `NEW-TOKEN (proposed)`. Never present a derived value as if it matched an existing system that wasn't given to you.
- **Conflicting spacing or color across two references of the same screen.** Flag the conflict in the relevant section with both values named. Do not average, and do not pick one without saying you picked one and why.
- **Low-resolution or cropped screenshot.** Mark affected values `(estimated)`. If the uncertain value would change an implementation decision (a token match, a contrast ratio near the WCAG threshold), add it to open questions instead of stating it as fact.
- **HTML input with both inline styles and CSS classes on the same element.** Prefer the computed/rendered style over a raw inline attribute when they conflict; note the conflict in section 3 if it affects a mapped token.
- **A component that only appears once and has no clear interactive affordance** (e.g., a static badge). Skip section 4 for it entirely rather than filling in eight rows of `not applicable`.

## After writing

State plainly at the end of your reply: how many spec files you wrote, how many open questions total, and how many of those are `BLOCKING`. That count is the real signal for whether this screen is ready to build.
