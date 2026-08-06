# Changelog

## [1.1.0] - 2026-08-06

- Theme variants handled: a screen supplied in light and dark is one screen and one spec file. Section 3 splits the observed-value column per theme, a value present in only one theme is a `NEW-TOKEN` rather than a conflict, and section 6 checks contrast once per theme.
- `references/handoff-template.md` documents the per-theme token-map columns.

## [1.0.0] - 2026-07-12

- Initial release: the eight-section handoff spec format (screen summary, layout, token map, component inventory, behavior notes, accessibility annotations, assets list, open questions).
- The no-invention rule as the core instruction: any state or value not visible in the source becomes a numbered, blocking/non-blocking open question instead of a guess.
- `references/handoff-template.md` added as a copy-pasteable spec skeleton.
- CI workflow validates `SKILL.md` frontmatter and confirms `README.md` links to it.
