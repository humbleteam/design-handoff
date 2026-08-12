# Changelog

## [1.2.0] - 2026-08-12

- Section 8 now collects open questions from sections 2 through 7, not 2 through 6. Section 7 was outside the range while still asking for values a mockup cannot show, so a question raised there had no numbered home and got written as a confident spec line instead - the exact failure the no-invention spine exists to prevent.
- Section 7 separates its recommendation from its assumptions. Format stays a recommendation. The `@1x/@2x/@3x` ladder follows from the target platforms, which a mockup does not name, so it is written as a stated default with an open question on the platforms. Whether an element is an exported asset or drawn in CSS gets an open question too, since dropping the row reads as "no asset needed", a decision the source never made.
- `references/handoff-template.md`: the section 7 table showed `@1x/@2x/@3x` pre-filled as though it were observed. It now carries a stated-default row and a CSS-versus-asset row, plus two notes on what section 7 cannot settle.

## [1.1.0] - 2026-08-06

- Theme variants handled: a screen supplied in light and dark is one screen and one spec file. Section 3 splits the observed-value column per theme, a value present in only one theme is a `NEW-TOKEN` rather than a conflict, and section 6 checks contrast once per theme.
- `references/handoff-template.md` documents the per-theme token-map columns.

## [1.0.0] - 2026-07-12

- Initial release: the eight-section handoff spec format (screen summary, layout, token map, component inventory, behavior notes, accessibility annotations, assets list, open questions).
- The no-invention rule as the core instruction: any state or value not visible in the source becomes a numbered, blocking/non-blocking open question instead of a guess.
- `references/handoff-template.md` added as a copy-pasteable spec skeleton.
- CI workflow validates `SKILL.md` frontmatter and confirms `README.md` links to it.
