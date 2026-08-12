<div align="center">

<h1>Design handoff</h1>

**Turns a finished mockup or HTML prototype into a dev-ready design handoff spec - a token map, a full component-state inventory, and accessibility notes an engineer can build from without pinging you.**

[![CI](https://img.shields.io/github/actions/workflow/status/humbleteam/design-handoff/validate.yml?branch=main&style=for-the-badge&logo=github&label=CI)](https://github.com/humbleteam/design-handoff/actions/workflows/validate.yml)
[![GitHub stars](https://img.shields.io/github/stars/humbleteam/design-handoff?style=for-the-badge&logo=github&color=181717)](https://github.com/humbleteam/design-handoff/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/humbleteam/design-handoff?style=for-the-badge&color=339933)](https://github.com/humbleteam/design-handoff/commits/main)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-skill-D97757?style=for-the-badge&logo=anthropic&logoColor=white)](https://github.com/humbleteam/design-handoff/blob/main/SKILL.md)

</div>

Design handoff is a Claude Code skill that reads a mockup, screenshot, or HTML prototype and writes a markdown spec: dimensions, layout measurements, a token map, component states, accessibility annotations, and open questions. The output is one file per screen. The rule that makes it useful: anything not visible in the source becomes an open question, never a guess.

## Table of contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Usage](#usage)
- [Example output](#example-output)
- [How it works](#how-it-works)
- [How is this different from just asking the model?](#how-is-this-different-from-just-asking-the-model)
- [FAQ](#faq)
- [Related skills](#related-skills)
- [Who maintains this](#who-maintains-this)

## What it does

- Writes one markdown handoff spec per screen from a mockup image, screenshot, or HTML file.
- Maps every color, font, spacing value, radius, and shadow to an existing design token, or flags it `NEW-TOKEN`.
- Enumerates the eight canonical component states - default, hover, focus, active, disabled, loading, empty, error - flagging whatever the source doesn't show.
- Writes accessibility annotations: landmark roles, accessible names, a focus order, and WCAG 2.2 contrast ratios.
- Lists every asset to export, with a recommended format and size - and raises an open question where the source cannot settle the density ladder or whether an element is an asset at all.
- Closes every spec with a numbered, blocking/non-blocking open-questions list.

## Quick start

**Personal (all projects):**

```bash
git clone https://github.com/humbleteam/design-handoff ~/.claude/skills/design-handoff
```

**Project only (this repo):**

```bash
git clone https://github.com/humbleteam/design-handoff .claude/skills/design-handoff
```

**Other agents (Cursor, Codex, or any LLM agent):** the skill is plain markdown in the [Agent Skills](https://github.com/humbleteam/design-handoff/blob/main/SKILL.md) format. Paste `SKILL.md` into the agent's system prompt or custom-instructions field.

Restart Claude Code, then run `/skills` (or check `~/.claude/skills/` or `.claude/skills/` directly) to confirm `design-handoff` is listed.

## Usage

- "Here's a screenshot of our checkout screen - write a handoff spec for it." Claude walks the layout region by region and produces a token map and state table for the pay button and promo-code field.
- "This is `pricing.html` and our `tokens.css` - write the handoff spec against our real tokens." Claude maps each value to an existing CSS custom property and flags unmatched values as `NEW-TOKEN`.
- "This mockup only shows the toggle's default state - document what we know and flag the rest." Claude fills in the default row, marking hover/focus/active/disabled as open questions.

## Example output

Excerpt from a spec for a fictional pricing card, default state only, no token set supplied:

```markdown
# Handoff spec: Northwind - pricing card (desktop)

**Source:** pricing-card.png (1 screenshot, default state only)
**Date:** 2026-07-12
**Status:** Draft - 3 open questions, 1 blocking

## 1. Screen summary
- Dimensions: 360 x 480 px (desktop, single card)
- Purpose: Displays one pricing tier with a primary CTA.

## 3. Token map
| Property | Observed value | Mapped token | Status |
|---|---|---|---|
| Card background | #FFFFFF | - | NEW-TOKEN: `surface` (proposed, no token set supplied) |
| CTA fill | #2563EB | - | NEW-TOKEN: `primary` (proposed, no token set supplied) |
| Card corner radius | 12px | - | NEW-TOKEN: `radius-lg` (proposed) |

## 4. Component inventory
Source shows default state only - table below is mostly open questions by design.

### CTA button ("Choose plan")
| State | Visible in source? | Spec |
|---|---|---|
| Default | yes | fill `primary`, label 15px/600, 12px vertical padding |
| Hover | no | OPEN QUESTION #1 |
| Focus | no | OPEN QUESTION #2 |
| Disabled | no | not applicable - plan is always selectable in this flow |

## 8. Open questions
1. [BLOCKING] CTA hover fill not shown in source - need the color before implementation.
2. [NON-BLOCKING] CTA focus ring not shown - proposing a 2px `primary` outline as a platform default; confirm before ship.
3. [NON-BLOCKING] No token set was supplied - all three tokens above are proposed, not confirmed against a real system.
```

## How it works

- **The no-invention rule is the spine.** A value or state not visible in the source becomes a numbered open question.
- **One spec per screen.** Same-layout images with only a state changed count as one screen; different screens get separate files.
- **Layout is walked in reading order**, with spacing marked `(estimated)` when estimated from proportions, not measured.
- **Token matching uses a tolerance.** A value within a few pixels, or a close color match, counts as that token; anything else is `NEW-TOKEN`.
- **Component states are checked against a fixed list of eight**: default, hover, focus, active, disabled, loading, empty, error.
- **Accessibility annotations cite WCAG 2.2 success criteria** by number (SC 1.4.3 text contrast, SC 1.4.11 non-text UI components).
- **Conflicting evidence is flagged, not resolved.** Two screenshots with different padding for one region produce a documented conflict, not an average.
- **Every open question is tagged blocking or non-blocking**, so a reviewer can tell if the spec is ready to build.
- **The assets list is held to the same standard as the rest.** A raster asset's `@1x/@2x/@3x` ladder follows from the target platforms, and a mockup does not name them; a divider or chevron may be an exported file or pure CSS. Both become numbered open questions rather than confident spec lines.

## How is this different from just asking the model?

A bare prompt like "write a dev spec for this screen" produces clean prose that fills gaps: a plausible hover color, a disabled state nobody showed you. It skips contrast ratios, focus order, and asset sizes, because nothing asks for them by name. This skill pins a fixed section order, forces a match-or-flag decision for every token, and turns every unseen state into a counted, tagged question. The output is a file a developer can check off, not a paragraph to re-verify against the mockup.

## FAQ

**What should a design handoff include?**
Exact dimensions, layout measurements, a token map, all component states, behavior notes, accessibility annotations, an asset export list, and open questions - eight sections.

**How do I write a design spec for developers?**
Separate what's visible - measurements, colors, states - from what you're assuming, and keep assumptions in a numbered open-questions section, not spec lines.

**How do I document component states?**
Check every interactive element against a fixed list: default, hover, focus, active, disabled, loading, empty, error. Most mockups only show 2-3; mark the rest as open questions.

**What is a design handoff checklist?**
Dimensions, layout measurements, token mapping, all eight component states, accessibility roles and contrast, an asset list, and open questions. `references/handoff-template.md` in this repo is that checklist.

**Can Claude generate a design handoff spec from a screenshot?**
Yes - paste the screenshot with a prompt like "write a handoff spec for this screen". Dimensions get marked `(estimated)` when the screenshot lacks exact pixel data.

**What's the difference between a design handoff spec and a style guide?**
A style guide documents a design system's tokens in general. A handoff spec documents one screen against that system: which tokens it uses, what's unresolved.

**How many states should I document for a button?**
Up to eight - default, hover, focus, active, disabled, loading, empty, error - though a button with no loading behavior skips that row. The point is deciding each state, not hitting a count.

## Related skills

Part of a 10-skill open-source kit for design teams by Humbleteam.

- [design-review](https://github.com/humbleteam/design-review) - structured UX critique with a 0-4 score, Before/After/Why fixes, and a citation for every claim.
- [ascii-wireframes](https://github.com/humbleteam/ascii-wireframes) - three distinct layout hypotheses as ASCII wireframes before any hi-fi work.
- [html-mockup](https://github.com/humbleteam/html-mockup) - census-first HTML mockups that match a reference screenshot: exact palette, item counts, component states.
- [extract-design-tokens](https://github.com/humbleteam/extract-design-tokens) - pull palette, type, spacing, radii, and shadows from a URL or screenshot into CSS variables and JSON.
- [audit-design-tokens](https://github.com/humbleteam/audit-design-tokens) - find token drift in a codebase: raw hex values, off-scale spacing, near-duplicate colors.
- [design-qa](https://github.com/humbleteam/design-qa) - a pre-ship design QA gate: states, contrast, touch targets, breakpoints, keyboard paths.
- [accessibility-audit](https://github.com/humbleteam/accessibility-audit) - WCAG 2.2-grounded accessibility review with success-criterion citations and severity levels.
- [ux-writing](https://github.com/humbleteam/ux-writing) - interface copy that reads human: plain-verb microcopy rules and an AI-tell strip pass.
- [design-brief](https://github.com/humbleteam/design-brief) - extract a 5-bullet design brief from messy project inputs, with a gap report for what is missing.

## Who maintains this

[Humbleteam](https://humbleteam.com/) is a digital product design and AI-engineering studio: founded in 2017, working from Prague and Dubai, with 80+ digital awards to the name, including 14 Awwwards wins, a Webby, and a Red Dot. We design digital products for startups and enterprises in fintech, healthtech, sports, and AI, and we build AI infrastructure for design teams - agents, workflows, and skills like this one.

This skill is distilled from the internal playbooks we run on client work: the same checklists behind the case studies at [humbleteam.com/work](https://humbleteam.com/work), for clients like Tinder and Acronis.

- The full 10-skill kit: [Related skills](#related-skills) above, or all repos at [github.com/humbleteam](https://github.com/humbleteam)
- What we do with AI for design teams: [humbleteam.com/ai](https://humbleteam.com/ai)
- Design and AI writing: [humbleteam.com/blog](https://humbleteam.com/blog)
- LinkedIn: [linkedin.com/company/humbleteam](https://www.linkedin.com/company/humbleteam/)
- Talk to us: [hi@humbleteam.com](mailto:hi@humbleteam.com)

Issues and PRs welcome.

MIT - see [LICENSE](LICENSE).
