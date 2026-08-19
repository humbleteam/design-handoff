# Handoff spec template

Read this when you are about to write a spec file. Copy the skeleton below, fill in every bracketed placeholder, and delete this instruction line and the notes in parentheses. Keep the eight `##` section headers and their order exactly as written - that order is the contract, not a suggestion.

```markdown
# Handoff spec: <Screen name>

**Source:** <file name(s), URL, or screenshot description - list every input that fed this spec>
**Date:** <YYYY-MM-DD>
**Status:** Draft - <N> open questions, <N> blocking

## 1. Screen summary
- Dimensions: <width> x <height> px (<breakpoint name>) <(estimated) if not measured directly>
  (one line per breakpoint when the spec covers more than one)
- Purpose: <one sentence - what this screen does and for whom>
- Sources: <N> image(s) / file(s)

## 2. Layout
(List every region top to bottom, reading order. One line each.)
- <Region name>: <position/size>, padding <value>, gap to next region <value> <(estimated) if applicable>
- <Region name>: ...

(If two sources claiming the same theme and breakpoint disagree on spacing, note the conflict here explicitly - do not average. A value that changes across breakpoints is a variant, not a conflict; it belongs in the section 3 columns.)

## 3. Token map
| Property | Observed value | Mapped token | Status |
|---|---|---|---|
| <e.g. Card background> | <e.g. #FFFFFF> | `<token-name>` or `-` | existing / NEW-TOKEN: `<proposed-name>` (proposed) |

(One row per distinct color, font, spacing value, radius, and shadow on the screen. No repeats - one row per property even when the spec carries two themes or two breakpoints; the observed-value column splits instead.)

## 4. Component inventory
(One subsection per distinct interactive component. If the source shows only one state for the whole screen, say so in a line before the first table.)

### <Component name>
| State | Visible in source? | Spec |
|---|---|---|
| Default | yes/no | <fill, label, border, icon - or `OPEN QUESTION #n`> |
| Hover | yes/no | ... |
| Focus | yes/no | ... |
| Active | yes/no | ... |
| Disabled | yes/no | ... |
| Loading | yes/no | ... |
| Empty | yes/no | ... |
| Error | yes/no | ... |

## 5. Behavior notes
- Transitions: <only what's shown or stated, else `OPEN QUESTION #n`>
- Scroll behavior: <sticky/infinite/parallax, or `OPEN QUESTION #n`>
- Validation timing: <on blur / on submit / on keystroke, or `OPEN QUESTION #n`>

## 6. Accessibility annotations
- Landmark roles: <header, nav, main, list, dialog, etc.>
- Accessible names: <proposed name per control>
- Focus order: 1) <element> 2) <element> ...
- Contrast pairs checked:
  - <foreground> on <background> = <ratio> (WCAG 2.2 SC 1.4.3 - needs 4.5:1 normal text / 3:1 large text; SC 1.4.11 for non-text UI components)

## 7. Assets list
| Asset | Format | Size(s) | Notes |
|---|---|---|---|
| <e.g. hero-icon> | SVG | single | |
| <e.g. avatar-photo> | WebP | @1x/@2x/@3x (default) | Target platforms not stated - `OPEN QUESTION #n` |
| <e.g. card-divider> | `OPEN QUESTION #n` | - | May be a 1px CSS border rather than an exported asset |

## 8. Open questions
1. [BLOCKING / NON-BLOCKING] <question, tied to the section it came from>
2. [BLOCKING / NON-BLOCKING] <question>
```

## Notes on filling this in

- Section numbers in the open-questions list must match the `OPEN QUESTION #n` references used earlier in the spec. Section 7 counts: questions raised in the assets list are collected in section 8 alongside every other one.
- Section 7 holds two values a mockup almost never states - the density ladder, which follows from the target platforms, and whether a visual element is an exported asset or drawn in CSS. Write the default and raise a question rather than presenting either as observed.
- A component with no plausible interactive state (a static badge, a label) does not get a subsection in section 4 - skip it rather than filling eight rows with `not applicable`.
- If no token set was supplied for this screen, every row in section 3 reads `NEW-TOKEN` and the spec should say near the top that none of the proposed tokens are confirmed against an existing system.
- If the screen was supplied under more than one declared condition - two themes, or two breakpoints - split "Observed value" in section 3 into one column per condition (`Observed (light)` / `Observed (dark)`, or `Observed (375)` / `Observed (1440)`) and keep one row per property. Still one spec file, not one per condition. A value that differs across conditions is a variant, not conflicting evidence; the conflict rule fires only when two sources claim the same condition and disagree. Section 6 lists a contrast ratio per theme for each pair, and breakpoints need no extra check because they do not change color.
