# Changelog

## [1.5.0] - 2026-09-17

- The FAQ gave the fixed eight states away in one answer. "How many states should I document for a button?" read "up to eight ... though a button with no loading behavior skips that row", which contradicts Step 4 on both halves: the roster is enumerated exactly, not up to a ceiling, and a row has exactly three legal marks - a spec line, `not applicable` with a one-clause reason, or a numbered open question. Skipping is a fourth, and it is the move 1.3.1 removed from the worked example on 2026-09-07, so the README went on granting in prose what the example had been fixed for.
- The skip wording had been borrowed from a rule one level up. A whole component with no plausible interactive state is skipped - a static badge gets no table, per Edge cases and the template's third filling-in note - and that is what makes the roster affordable. A row inside a table is never skipped, because a row nobody answered reads exactly like a state nobody needs.
- The answer's own premise was the invention the spine exists to stop. "A button with no loading behavior" is a flow fact, and a static mockup does not carry it; the repo's example says so two screens earlier, where the unshown loading state is open question 5 asking whether choosing a plan blocks on a request. The answer now names the three marks, says there is no fourth, and gives that non-answer as the example of what is not one of them.
- "How do I document component states?" ended "mark the rest as open questions", dropping the `not applicable` mark the same example uses for Empty. It now names both, with the test between them: a reason has to be readable off the artifact, and a reason that needs a fact the source never showed is an invention rather than a dismissal.

## [1.4.0] - 2026-09-12

- The declared-condition rule stopped one section short. 1.3.0 generalized theme and breakpoint into one rule and named the sections that carry it - 1 takes a dimensions line per breakpoint, 3 splits its observed-value column, 6 checks contrast per theme - while section 4 was left with a single `Visible in source?` column and a single spec line, and a component's fill and padding are exactly the values a theme and a breakpoint change.
- What that left with no legal answer: a CTA screenshotted in light and dark. One spec line holds one fill, so the writer either dropped a value the source gave - the failure 1.3.0 fixed for section 1 - or wrote the light fill as the spec for both, which is the invention the spine exists to stop, in the section where it looks most like a measurement.
- Section 4 now splits both columns per condition and keeps one row per state, the same shape section 3 uses. A state seen under one condition reads `yes` / `no` and its spec line covers only that condition; the other condition is an `OPEN QUESTION`, never the first one's values reused; a state identical under every condition supplied is written once with those conditions named, because one line for one observation is accurate and a copied line claims a reading that never happened.
- The Output paragraph's roster of sections that carry a declared condition now names 4 alongside 1, 3, and 6, so the list matches the rules underneath it.
- `references/handoff-template.md`: the section 4 table carries the per-condition note, and the filling-in note on declared conditions names section 4. README: the mobile-and-desktop answer says the state table splits too.

## [1.3.1] - 2026-09-07

- README example: the CTA button's state table listed four of the eight canonical states, dropping active, loading, empty, and error with no mark of any kind. Step 4 requires all eight enumerated for every interactive component, each ending in a spec line, a `not applicable` with a one-clause reason, or a numbered open question - so the worked example showed the fixed roster being shortened, which is the move the fixed roster exists to prevent.
- The `Disabled` row read `not applicable - plan is always selectable in this flow`. One default-state screenshot of one card cannot show what the flow does with an already-current plan or a sold-out tier, so that reason asserted a fact the source never carried - the invention the spine exists to stop, written into the example that teaches the spine. Disabled is now an open question. The one `not applicable` left is `Empty`, whose reason is readable from the artifact itself: a button with a fixed label holds no content that can be empty.
- Open questions renumbered to 7, and the Status line now says 7. The old count of 3 was only correct because half the roster was missing, so the two errors had been hiding each other.

## [1.3.0] - 2026-08-19

- A spec covering two breakpoints had no legal token map. The output rules explicitly keep multiple breakpoints of one screen in one file, with a subsection per breakpoint under section 2, while section 3 kept a single "Observed value" column and required every property to appear "exactly once each". A card padding of 16px at 375 and 32px at 1440 therefore had no way to be recorded: two rows break the no-repeats rule, one row drops a measurement the source actually gave, and the only nearby rule - flag conflicting sources - misfiles an intentional responsive step as a discrepancy and sends the developer back with a question that has no answer.
- Generalized the per-theme handling added in 1.1.0 into one rule for declared conditions, theme and breakpoint alike: one column per condition in section 3, one row and one token name per property, a differing value is a variant and a one-condition value is a `NEW-TOKEN` for that condition.
- Defined a conflict, which was previously left to the reader: two sources claiming the same condition and disagreeing. A difference across a declared condition is not one, so the conflict rule no longer swallows responsive and theme variation.
- Section 1 records one named dimensions line per breakpoint. One line cannot describe two viewports, and picking one silently drops a measured value.
- `references/handoff-template.md` carries the same rule in all three places it appears, and the README adds an FAQ answer on covering mobile and desktop in one spec.

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
