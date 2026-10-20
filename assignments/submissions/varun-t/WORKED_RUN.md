# Worked Run — OPT-Aware Frontend Job Opportunity Evaluator

## Executive summary

**What this is.** The record of one run of the frontend-role evaluator on four
invented companies, with the real output, what was and was not established, a
deliberate attempt to break it, and what went wrong along the way.

**Why read it.** It shows which claims rest on evidence a script produced and
which rest on assumption, so you can judge how far to trust the procedure.

**What it found.** The scorer gave 1 Apply, 1 Consider and 2 Skip (skip rate
50%). The two Skips came entirely from closed gates. A deliberate break of the
gate rule was caught by 5 of 5 checks. **All role data was synthetic, so this
verifies the arithmetic and the gate behavior. It does not verify that any real
posting is open or that anyone is eligible.** One earlier attempt failed because
the wrong file was used as input.

## Run record

| Field | Value |
|---|---|
| Recipe | `recipes/case-frontend-opt-sponsorship.md` v0.1.0 |
| Date | 2026-10-20 |
| Scenario | synthetic: four fictional companies, no real person or posting |
| Input | `assignments/submissions/varun-t/frontend-opt-sample.json` (sha1 `108e148b…`) |
| Outputs | `sample-output/role-scores.json` (sha1 `cab668ce…`), `role-scores.md` (sha1 `c688e801…`) |

Provenance of this document: the scorer output below was **read from the existing
files** in this session (not regenerated). The harness and verify output below
was **re-run in this session** on 2026-10-20. The original scoring command line
was not captured in a log, so it is not reproduced; the recipe lists the
repeatable form.

## Verification artifacts (copied)

The harness writes to the gitignored `output/`, so two of its files are copied
into this submission unchanged (verified byte-identical with `cmp`):

| Copy | Original source path | sha1 |
|---|---|---|
| `verification/gate-harness-2026-10-20.json` | `output/gate-harness/gate-harness-2026-10-20.json` | `1af7a9a7…` |
| `verification/gate-harness-2026-10-20-audit.md` | `output/gate-harness/gate-harness-2026-10-20-audit.md` | `85e178fa…` |

The harness names files by date, so the re-run on 2026-10-20 during this
submission audit overwrote the files from the earlier run that day. These copies
are from that **latest** re-run, not the first. The user-reported result of the
first run is the same as what these files show: `PASS`, 10 baseline cases, 3
structural checks, 5 caught and 0 missed in the negative control. The copied
JSON also records the one warning, `P1-role-quality-inert`.

## Inputs (inspected)

Four records, all with `source` labels beginning `synthetic-`:

| role_id | Company | sponsorship p / tier | fit p | role_quality p | liveness | timeline |
|---|---|---|---|---|---|---|
| frontend-clear | Fictional Company A | 0.9 / proven | 0.85 | 0.8 | 1 | 1 |
| fullstack-closed | Fictional Company B | 0.95 / proven | 0.9 | absent | 0 | 1 |
| frontend-timeline | Fictional Company C | 0.9 / proven | 0.9 | absent | 1 | 0 |
| frontend-uncertain | Fictional Company D | 0.6 / likely | 0.85 | absent | 1 | 0.9 |

## Output (read from `role-scores.json`)

Weights in the output: sponsorship 0.35, fit 0.3, role_quality 0; threshold 0.3;
consider floor 0.2; gate-zero 0.05; profile needs sponsorship: true.

| role_id | Composite | Recommendation | Arithmetic line (verbatim from trace) |
|---|---|---|---|
| frontend-clear | 0.57 | Apply | `(0.9·0.35 + 0.85·0.3 + 0.8·0) × 1 × 1 = 0.570` |
| frontend-uncertain | 0.4185 | Consider | `(0.6·0.35 + 0.85·0.3) × 1 × 0.9 = 0.418` |
| fullstack-closed | 0 | Skip | `(0.95·0.35 + 0.9·0.3) × 0 × 1 = 0.000` |
| frontend-timeline | 0 | Skip | `(0.9·0.35 + 0.9·0.3) × 1 × 0 = 0.000` |

Summary line from `role-scores.md`: *4 roles → Apply 1 · Consider 1 · Skip 2.
Skip rate 50%.* The JSON stores 0.4185; the Markdown shows 0.418 (display
rounding), not a discrepancy in the decision.

Reasons printed by the scorer: frontend-uncertain was demoted from a would-be
Apply because its sponsorship tier is "likely" (a soft spot); the two Skips are
"gated" by liveness ≈ 0 and timeline ≈ 0 respectively.

## Verified vs. inferred

| Statement | Kind | Basis |
|---|---|---|
| 4 roles scored; Apply 1 / Consider 1 / Skip 2; skip rate 50% | Verified | read from output files |
| Composite = vote sum × both gates for each role | Verified | arithmetic lines above; harness arithmetic-identity check 10/10 on its own cases |
| Closed gates zero the composite despite strong votes | Verified | rows B and C; harness cases G1, G2 |
| role_quality had no effect on frontend-clear | Verified | value 0.8 × weight 0 = 0 contribution in the trace |
| Gate-as-vote mutation is caught | Verified | negative control, 5 of 5 |
| All sponsorship, fit, liveness, timeline values | **Synthetic** | typed into the sample file |
| `fit` values | **Model judgment (synthetic)** | labeled `synthetic-model-judgment` in the trace |
| Company A "would be a good application" | **Not established** | fictional company; no real posting |
| Any posting is open; any person is OPT-eligible | **Not established** | nothing in the run checks either |
| Sponsorship tier labels (proven / likely) | **Assumed** | typed in; not read from `data/80-days-to-stay/` |

## Commands and real output (re-run this session)

```
$ npm run verify
conformance: 158 files (85 md · 36 py · 30 js · 4 sh · 3 json)
✓ all conform (machine half of P4). Adequacy is still the human gate.
WARN (3):
  W1 ignore path not in .gitignore: archive/
  W2 private path not gitignored (PII/secret risk): private/
  W2 private path not gitignored (PII/secret risk): data/ats/
✓ manifest check passed (3 warnings)
```

```
$ npm run score:gates
      negative control: 5/5 gate-sensitive assertions correctly failed under mutation
config drift      : none
baseline cases    : 10 passed, 0 failed  (of 10)
structural checks : 3 passed, 0 failed  (of 3)
paired checks     : 1 warning(s)
negative control  : PASS — mutation caught by 5 assertion(s)
RESULT            : PASS
```

`npm run doctor` ended with: `environment: ✓ runnable`, `recipes: 33/33 carry
lifecycle frontmatter — all tracked`. (Counts were taken before the new recipe
was added.)

The three `verify` warnings are about ignore paths and are repository-level, not
caused by this submission. W2 is worth noting on its own: the manifest check
reports `private/` and `data/ats/` as not matched in `.gitignore` by its test.
That was not investigated here.

## Negative control

The harness copies the scorer to a temp directory and applies two mutations: it
makes the composite additive (`voteSum + liveness*0.20 + timeline*0.15`) and
removes the closed-gate branch. No repository file is modified. All five
gate-sensitive cases passed at baseline and failed under mutation:

| Case | Baseline | Under mutation |
|---|---|---|
| G1 liveness zero, maximal votes | PASS | FAIL (composite 0.8, Apply) |
| G2 timeline zero, maximal votes | PASS | FAIL (composite 0.85, Consider) |
| G3 gate at boundary | PASS | FAIL (composite 0.81, Apply) |
| G4 gate just above boundary | PASS | FAIL (composite 0.812, Apply) |
| G5 two partial gates | PASS | FAIL (composite 0.825, Consider) |

This tests the **harness's own fixtures**, not the four sample roles.

## Unresolved issues surfaced by the run

- **Role-quality weight is 0.** The harness warning `P1-role-quality-inert` confirms moving role quality from 0.0 to 1.0 left the composite unchanged (0.325 vs 0.325). In the sample, frontend-clear carried a role_quality of 0.8 that contributed nothing. This is an open authorial decision (DOMAIN.md gap 3), not a bug.
- **Missing inputs have defaults.** Three of four records omit `role_quality` (skipped silently). A missing `liveness` or `timeline` would be treated as 1, an open gate. No sample record omits a gate, so this default was **not exercised**; it is read from the scorer source.
- **Sponsorship uncertainty.** Company D's "likely" tier (0.6) demotes it to Consider, but the 0.6 and the tier label are both invented. A tier label that is absent cannot trigger the demotion.
- **Synthetic gates.** The liveness and timeline factors were typed in. Nothing checked a live posting or an OPT end date.

## Failed and earlier commands

| ID | What was tried | What happened | Status |
|---|---|---|---|
| F1 | Scored `data/examples/role-scores.json` as input | User reported zero composites. **Reproduced this session** (output to a scratch directory, nothing in the repo written): exit 0, `✓ scored 5 roles → Apply 1 · Consider 0 · Skip 4 (skip 80%)`. All five rescored composites were 0 (originals: 0.4462, 0.1785, 0, 0.4185, 0.1934). **Cause:** that file is a scorer output, not role records. The scorer accepts a bare array or an object with a `roles` array, so it read the file without complaint, but the records have no `sponsorship`, `fit`, `liveness` or `timeline` fields, so no votes were found. The "Apply 1" is `override-demo`, whose old override field carried through and replaced the machine's Skip, at a composite of 0. | Discovered input-contract failure; guard proposed as a DEV TODO; not fixed in the scorer |
| P1 | Sample scoring run | Succeeded, outputs in `sample-output/` | Existing evidence |
| P2 | JSON validation, gate harness (10 + 3 passed, 5/5 caught), `verify`, `doctor` | User reported pass; **harness and verify were re-run this session with the same results** | Re-confirmed |

F1 matters because it was a **silent** failure: the scorer exited 0 and produced
plausible-looking, wrong zeros, and it even reported a healthy-looking 80% skip
rate. A zero is also what a closed gate produces, so it can be mistaken for a
legitimate Skip. Because the scorer treats missing `liveness`/`timeline` as 1,
these zeros came from missing votes, not from closed gates.

## Reflection

**Went well.** Gate behavior is tested by a script, with a negative control that
shows the test can fail. The two Skips are explainable term by term.

**Missed or wrong.** No real data entered the run. The sponsorship dataset and the
liveness script were available and unused. The role-quality signal is wired in
but inert. The F1 failure shows the scorer has no guard against wrong input.

**Next steps.** (1) Run one realistic scenario with sponsorship values read from
`data/80-days-to-stay/` and liveness from `npm run ats:liveness`, keeping any
personal data in `private/`. (2) Decide the role-quality weight. (3) Add the input
guard. (4) Have a named human clear the live gates.

## Attestation

- Recipe: case-frontend-opt-sponsorship v0.1.0
- By: **not attested.** No human has reviewed or signed this run. Name and date to be added by the person who does. (Prepared by Varun T with Claude Code assistance, 2026-10-20; that is authorship, not sign-off.)

### Tested

| Ran | Saw | Expected |
|---|---|---|
| Read `role-scores.json` and `.md` | 4 roles; Apply 1, Consider 1, Skip 2; skip rate 50% | the observed result stated by the author |
| Recomputed each arithmetic line by hand from the input values | all four match the trace (0.570, 0.418, 0, 0) | composite = vote sum × gates |
| `npm run score:gates` | 10/10 baseline, 3/3 structural, negative control 5/5, `RESULT: PASS`, one WARN | PASS |
| **Deliberate break:** gate mutation inside the harness (additive gates, closed-gate branch removed) | all 5 gate-sensitive cases flipped from PASS to FAIL | mutation detected |
| **Deliberate break (unplanned, then reproduced):** scorer-output file used as input (F1) | exit 0, five zero composites, one carried-over "Apply", no error or warning | refusal or a warning |
| `npm run verify` | conformance passed; 3 manifest warnings | pass |
| `npm run doctor` | environment runnable | runnable |

The F1 row's observed result did **not** meet its expectation.

### Did not test

- Any real posting, real company, or real sponsorship record.
- `npm run ats:liveness`, `npm run ats:scan`, or any network-touching script.
- A record with `liveness` or `timeline` absent (the default-to-open behavior).
- Roles with a non-zero role-quality weight.
- Derivation of `timeline` from an actual OPT/STEM OPT end date.
- Profiles that do not need sponsorship.
- An override with and without a reason, on the sample (covered only by harness case G8).
- Whether the weights and thresholds are appropriate.
- Whether F1's carried-over override is a defect or intended behavior; only observed.

### Broke during testing, fixed

- **F1: wrong input file produced silent zeros.** Fixed by using the correct synthetic input (`frontend-opt-sample.json`). The underlying weakness, that the scorer accepts scorer output without complaint, is **not fixed**; it is recorded as a DEV TODO.
- Nothing else broke in this session.
