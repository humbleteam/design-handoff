# Handoff spec template

Read this when you are about to write a spec file. Copy the skeleton below, fill in every bracketed placeholder, and delete this instruction line and the notes in parentheses. Keep the eight `##` section headers and their order exactly as written - that order is the contract, not a suggestion.

```markdown
# Handoff spec: <Screen name>

**Source:** <file name(s), URL, or screenshot description - list every input that fed this spec>
**Date:** <YYYY-MM-DD>
**Status:** Draft - <N> open questions, <N> blocking

## 1. Screen summary
- Dimensions: <width> x <height> px (<breakpoint name>) <(estimated) if not measured directly>
- Purpose: <one sentence - what this screen does and for whom>
- Sources: <N> image(s) / file(s)

## 2. Layout
(List every region top to bottom, reading order. One line each.)
- <Region name>: <position/size>, padding <value>, gap to next region <value> <(estimated) if applicable>
- <Region name>: ...

(If spacing conflicts across sources, note it here explicitly - do not average.)

## 3. Token map
| Property | Observed value | Mapped token | Status |
|---|---|---|---|
| <e.g. Card background> | <e.g. #FFFFFF> | `<token-name>` or `-` | existing / NEW-TOKEN: `<proposed-name>` (proposed) |

(One row per distinct color, font, spacing value, radius, and shadow on the screen. No repeats.)

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
| <e.g. avatar-photo> | WebP | @1x/@2x/@3x | |

## 8. Open questions
1. [BLOCKING / NON-BLOCKING] <question, tied to the section it came from>
2. [BLOCKING / NON-BLOCKING] <question>
```

## Notes on filling this in

- Section numbers in the open-questions list must match the `OPEN QUESTION #n` references used earlier in the spec.
- A component with no plausible interactive state (a static badge, a label) does not get a subsection in section 4 - skip it rather than filling eight rows with `not applicable`.
- If no token set was supplied for this screen, every row in section 3 reads `NEW-TOKEN` and the spec should say near the top that none of the proposed tokens are confirmed against an existing system.
- If the screen was supplied in more than one theme, split "Observed value" in section 3 into one column per theme (`Observed (light)`, `Observed (dark)`) and keep one row per property. Still one spec file, not one per theme. Section 6 lists a contrast ratio per theme for each pair.
