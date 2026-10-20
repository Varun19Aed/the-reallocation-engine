---
status: RUNNABLE-SAMPLE
todos_open: 6
last_gate: "sample-run, 2026-10-20, assignments/submissions/varun-t/RUN_LOG.md"
attestation: null
recipe_version: 0.1.0
type: case
---

# Case: OPT-Aware Frontend Job Opportunity Evaluator

## Executive summary

**What this is.** A repeatable procedure for an international student on OPT or
STEM OPT who is applying for frontend engineering jobs. For each candidate role
it combines four judgments (how likely the employer is to sponsor, how well the
role fits, whether the posting is still open, whether the start date works for
the work-permit clock) into one number and one recommendation: Apply, Consider,
or Skip.

**Why read it.** The two "can this even happen?" questions, whether the posting
is open and whether the timing works, are treated as hard stops, not as points
to be traded against enthusiasm. A great-fit role at a closed posting scores
zero. The procedure is also explicit about what it cannot know.

**What it found so far.** One run on four invented companies produced one
Apply, one Consider and two Skip (a 50% skip rate). The two Skips came from a
closed posting and an impossible start date, even though both had strong
sponsorship and fit scores. A test that deliberately breaks the scoring rule
caught the break in all five relevant checks. **Every input in that run was
made up, so it shows the arithmetic works. It does not show any real job is
worth applying to.** The recipe is therefore marked as runnable on sample data
only, and no human has signed it off.

## Run record

| Field | Value |
|---|---|
| Recipe | `case-frontend-opt-sponsorship` v0.1.0 |
| Lifecycle stage claimed | RUNNABLE-SAMPLE (evidence below) |
| Sample run date | 2026-10-20 |
| Author | Varun T (INFO 7375 Mode Build) |
| Human attestation | none recorded (`attestation: null`) |

## Purpose and when to use

Use when ranking a small batch (about 3–30) of frontend roles for an F-1/OPT or
STEM OPT candidate who needs sponsorship. Do **not** use it to decide on a single
role without checking the posting by hand, to estimate a person's chance of an
H-1B lottery win, or to give legal or immigration advice.

## Source inventory

| Item | Path / command | Status |
|---|---|---|
| Composite scorer (Ch.11) | `scripts/score/role-scorer.mjs`, `npm run score <roles.json>` | exists, ran |
| Gate harness + negative control | `scripts/score/gate-harness.mjs`, `npm run score:gates` | exists, ran |
| Gate fixtures | `scripts/score/fixtures/gate-cases.json` | exists, not modified |
| Sample input (synthetic, 4 roles) | `assignments/submissions/varun-t/frontend-opt-sample.json` | exists |
| Sample outputs | `assignments/submissions/varun-t/sample-output/role-scores.{json,md}` | exist |
| Sponsorship history (real) | `data/80-days-to-stay/` | **not used in the sample run** |
| Posting liveness (real) | `npm run ats:liveness -- <job-url>` | **not used in the sample run** |
| Role quality (real) | `data/bls/`, `npm run bls:local-wage` | **not used; weight is 0** |
| Conformance / manifest | `npm run verify`, `npm run doctor` | ran |

**Input contract.** `role-scorer.mjs` expects a JSON **array** of role records:
`role_id`, `company`, `title`, `sponsorship{p,tier,source}`, `fit{p,source}`,
optional `role_quality{p,source}`, `liveness{factor,source}`,
`timeline{factor,source}`. Its own output (an object with a `roles` array and a
`trace` per role) is **not** valid input, yet the scorer accepts it without an
error. Feeding `data/examples/role-scores.json` back in exited 0 with all
composites 0 (see the worked run, failed command F1).

## Proposed additions (typed TODOs)

- `[TODO][DEFINE]` Set the `role_quality` weight, or decide in writing that it stays 0, with one sentence of reasoning. It is currently `0.0`, so role quality changes no recommendation (DOMAIN.md gap 3; harness warning `P1-role-quality-inert`).
- `[TODO][DEFINE]` Confirm or replace `consider_floor = 0.20`, marked `[VERIFY]` in the scorer and not pinned by the chapter.
- `[TODO][DATA SOURCE]` Replace synthetic values with real ones for at least one realistic scenario: file at a named path plus a one-line provenance note (origin, date). Any real application data goes in `private/`, never a tracked file.
- `[TODO][DEV]` Add an input guard to the scorer or a wrapper that refuses scorer-output files and warns when `liveness` or `timeline` is absent (today absent means 1, an open gate). Proposed only; **does not exist**.
- `[TODO][DEFINE]` Define how `timeline.factor` is derived from an OPT/STEM OPT end date and the employer's start date. Today it is typed in by hand.
- `[TODO][APPROVE]` A named human clears the live-run gate (Gate 6) and attests, before the stage moves past RUNNABLE-SAMPLE.

## Repeatable commands

Run from the repository root.

```bash
# 1. Score a roles file (input must be a JSON array of role records)
npm run score -- assignments/submissions/varun-t/frontend-opt-sample.json \
  --out-dir <scratch-dir> --md <scratch-dir>/role-scores.md

# 2. Test the gate semantics and run the negative control
npm run score:gates          # exit 0 = pass, 1 = assertion/control failed, 2 = could not run

# 3. Conformance and environment
npm run verify
npm run doctor
```

Write scorer output to a scratch directory when re-running; the committed sample
outputs are evidence of the original run and are not overwritten.

## Phase gates

Each gate is a hard stop with a testable condition and a failure path. A human
clears the gates marked **(human)**; the machine only reports.

1. **Problem gate.** The request names the candidate's work-authorization situation and the roles being compared. Test: both are written in the run log. Failure: stop and ask.
2. **Input-contract gate.** The input parses as a JSON array and every record has `role_id`, `sponsorship.p`, `fit.p`. Test: `node -e 'const a=require("./<file>");if(!Array.isArray(a))process.exit(1)'`. Failure: stop. Do not score scorer-output files.
3. **Stored-script gate.** `scripts/score/role-scorer.mjs` and `npm run score:gates` resolve. Failure: stop; do not write a replacement scorer.
4. **Gate-behavior gate.** `npm run score:gates` reports `RESULT: PASS`, config drift none, negative control PASS. Failure: stop; report the run as inconclusive.
5. **Liveness gate (human).** Each role's `liveness.factor` comes from a check of the real posting, with the source named. Synthetic or unverified values are labeled as such and cannot support a live decision. Failure: the role is not scored for a live decision.
6. **Timeline gate (human).** Each `timeline.factor` is derived from the candidate's actual authorization end date and the role's start date, with the derivation shown. Failure: same as Gate 5.
7. **Verification gate.** The scorer exits 0, output JSON parses, and for every role `composite == vote_sum × gate_product`. Failure: halt; do not edit the input to make output look right.
8. **Logging gate.** An entry is recorded in a run log. Students do not edit `logs/RUN_LOG.md`, so the entry goes in the submission log. Test: `grep -q "case-frontend-opt-sponsorship" assignments/submissions/varun-t/RUN_LOG.md`.
9. **Human attestation gate.** A named human reads the audit and the output and attests. Test: `attestation:` frontmatter is non-null. Failure: the recipe stays at its current stage.

## What it can and cannot verify

**Can (machine):** the input parses; the composite equals the weighted vote sum times both gate factors; a closed gate (factor ≤ 0.05) gives Skip; each term in the output carries a source label; gate-as-vote regressions are caught by the harness.

**Cannot:**
- Whether any sponsorship probability, fit score, liveness factor or timeline factor reflects reality. In the sample run they are typed-in synthetic values.
- Whether a posting is open. No live check ran.
- OPT or STEM OPT eligibility, E-Verify status, cap-gap, or any legal question. Nothing in the scorer encodes immigration rules.
- Whether the weights (0.35 / 0.30 / 0) or thresholds (0.3 / 0.2 / 0.05) are right.
- Role quality, because its weight is 0.

**Defaults that hide uncertainty (read these):**
- A missing `liveness` or `timeline` factor is treated as 1, an **open** gate, with no warning.
- A missing `role_quality` is skipped silently; with weight 0 it would not matter anyway.
- A role with no `sponsorship.tier` can never be demoted to Consider for a soft tier.

## Stop conditions

Refuse to produce or report a recommendation (do not guess) when:
- the input is not a JSON array of role records, or looks like scorer output;
- `liveness` or `timeline` is absent for a role being scored for a live decision;
- the gate harness does not return PASS with a valid negative control;
- the `role_quality` weight is changed without re-running the harness and updating the fixtures by hand;
- the requester asks for immigration or legal conclusions;
- any real personal data would be written to a tracked file.

## Human review responsibilities

- Set or accept the role-quality weight and the Consider floor (the DEFINE TODOs).
- Check each posting's liveness and each start date against the authorization end date, and name the source.
- Read the human report before the JSON, and distrust a recommendation they cannot explain term by term.
- Decide whether to override a recommendation, with a written reason; an override with no reason is ignored by the scorer.
- Clear Gates 5, 6 and 9 by name and date in the log. No one has done so yet.

## Output contract

### Machine output (agent log)

File: `role-scores.json`. Top-level fields: `_scorer`, `_chapter`, `generated`, `config` (weights and thresholds), `profile_needs_sponsorship`, `roles[]`. Per role: `role_id`, `company`, `title`, `composite`, `recommendation`, `machine_recommendation`, `reason`, `override`, `trace{votes[], vote_sum, gates[], gate_product, arithmetic}`. Every vote and gate carries a `source` label. Reader: scripts and the next agent.

### Human report

File: `role-scores.md`. Opens with the summary line (counts and skip rate), then one row per role: composite, recommendation, plain-language reason, and the term-by-term audit. Reader: the candidate. Decision enabled: which roles to spend application effort on, and which to skip. A skip rate below 50% is a prompt to check whether the gates were genuinely evaluated.

## Log template (submission run log; maintainers may fold it into `logs/RUN_LOG.md`)

```markdown
### YYYY-MM-DD — case-frontend-opt-sponsorship · <sample|live>
- **Recipe:** case-frontend-opt-sponsorship v0.1.0
- **Inputs:** <roles file path>; <synthetic | real, source>
- **Commands:** npm run score ...; npm run score:gates; npm run verify
- **Outputs:** <paths>
- **Result:** N roles → Apply a · Consider b · Skip c; skip rate x%; harness result
- **Open issues:** <unresolved TODOs, defaults relied on, anything not tested>
- **Gates cleared by:** <name, date, or "none">
```
