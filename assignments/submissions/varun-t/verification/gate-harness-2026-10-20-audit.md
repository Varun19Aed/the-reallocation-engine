# Gate-Behavior Harness — audit (2026-10-20)

**Target:** `scripts/score/role-scorer.mjs` (Bayesian Role Scorer, Ch.11) · **Result: PASS**

Tests the Ch.11 claim that liveness and timeline are gates (multipliers), not votes (addends). Fixtures are **synthetic test vectors**, not records — no number in this report is a claim about a real company, posting, or person.

## Config

| Key | Fixture expects | Scorer reports | Match |
|---|---|---|---|
| weights.sponsorship | 0.35 | 0.35 | yes |
| weights.fit | 0.3 | 0.3 | yes |
| weights.role_quality | 0 | 0 | yes |
| apply_threshold | 0.3 | 0.3 | yes |
| consider_floor | 0.2 | 0.2 | yes |
| gate_zero | 0.05 | 0.05 | yes |

## Baseline cases

| Case | Result | composite (script-output) | vote_sum x gate_product | Recommendation |
|---|---|---|---|---|
| G1-liveness-zero-with-maximal-votes | PASS | 0 | 0.65 x 0 | Skip |
| G2-timeline-zero-with-maximal-votes | PASS | 0 | 0.65 x 0 | Skip |
| G3-gate-boundary-at-threshold | PASS | 0.0325 | 0.65 x 0.05 | Skip |
| G4-gate-boundary-just-above-threshold | PASS | 0.039 | 0.65 x 0.06 | Skip |
| G5-two-partial-gates-multiply | PASS | 0.1625 | 0.65 x 0.25 | Skip |
| G6-healthy-gates-strong-votes-apply | PASS | 0.555 | 0.555 x 1 | Apply |
| G7-soft-timeline-demotes-apply-to-consider | PASS | 0.3052 | 0.555 x 0.55 | Consider |
| G8-override-without-reason-is-ignored | PASS | 0.065 | 0.065 x 1 | Skip |
| G9a-role-quality-absent | PASS | 0.325 | 0.325 x 1 | Apply |
| G9b-role-quality-maximal | PASS | 0.325 | 0.325 x 1 | Apply |

## Structural checks

- **PASS** `arithmetic-identity` — composite == vote_sum x gate_product (tol 0.001) for 10/10 cases
- **PASS** `trace-source-labels` — every vote and both gates carry a source label for 10/10 cases
- **PASS** `both-gates-present` — liveness and timeline both appear as gates in 10/10 cases

## Paired checks (structural findings)

- **WARN** `P1-role-quality-inert` — composites 0.325 vs 0.325; equal: true. role_quality p moved 0.0 -> 1.0 with no change to the composite. This is expected while weights.role_quality = 0.0, and is the open authorial decision recorded in DOMAIN.md gap 3 — not a code defect. Reported so it cannot be forgotten.

## Negative control

Mutant: gates made additive (`voteSum + liveness*0.20 + timeline*0.15`) and the closed-gate branch removed. Written to an OS temp directory; **no repository file is modified**.

| Case | Baseline | Under mutation | Discriminates? |
|---|---|---|---|
| G1-liveness-zero-with-maximal-votes | PASS | FAIL (composite 0.8, Apply) | yes |
| G2-timeline-zero-with-maximal-votes | PASS | FAIL (composite 0.85, Consider) | yes |
| G3-gate-boundary-at-threshold | PASS | FAIL (composite 0.81, Apply) | yes |
| G4-gate-boundary-just-above-threshold | PASS | FAIL (composite 0.812, Apply) | yes |
| G5-two-partial-gates-multiply | PASS | FAIL (composite 0.825, Consider) | yes |

## What this harness cannot verify

- Whether the **input** numbers are true. It tests how the scorer combines sponsorship/fit/liveness/timeline values; it does not check that any of those values reflect reality. A ghost posting with `liveness.factor: 1.0` supplied by a broken upstream feed will be scored as live and this harness will not notice.
- Whether the **weights** are right. It asserts the config it was written against, so it detects drift — but it has no way to judge whether 0.35/0.30/0.0 are the correct weights.
- Whether the **thresholds** (0.30 / 0.20 / 0.05) are well chosen. Two of the three are marked `[VERIFY]` in the scorer itself.
- Anything about **real roles**. The fixtures are synthetic.

