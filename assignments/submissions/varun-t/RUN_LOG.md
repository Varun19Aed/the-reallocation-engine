# RUN_LOG entry — case-frontend-opt-sponsorship

## Executive summary

**What this is.** The log entry for one sample run of the OPT-aware frontend role
evaluator, in the format the course asks for.

**Why read it.** It is the short, factual record: what was run, what came out,
and what is still open.

**What it says.** Four invented roles were scored (1 Apply, 1 Consider, 2 Skip,
50% skip rate); the gate tests and the repository checks passed; one earlier
attempt with the wrong input file produced silent zeros. Nothing real was tested
and no human has signed off.

## Entry

### 2026-10-20 — case-frontend-opt-sponsorship · sample
- **Recipe:** case-frontend-opt-sponsorship v0.1.0 (claimed stage: RUNNABLE-SAMPLE)
- **Inputs:** `assignments/submissions/varun-t/frontend-opt-sample.json` (4 synthetic roles, no real data)
- **Commands:** role scorer on the sample (command line not captured at the time); `npm run score:gates`; `npm run verify`; `npm run doctor`
- **Outputs:** `assignments/submissions/varun-t/sample-output/role-scores.json`, `role-scores.md`; harness artifacts copied unchanged from gitignored `output/gate-harness/` to `assignments/submissions/varun-t/verification/` (from the latest same-day re-run; see WORKED_RUN.md)
- **Result:** 4 roles → Apply 1 · Consider 1 · Skip 2; skip rate 50%. Gate harness: 10/10 baseline, 3/3 structural, negative control 5/5 caught, exit PASS. `verify`: conformance passed, 3 manifest warnings (`archive/`, `private/`, `data/ats/` not matched by the manifest's .gitignore test). `doctor`: runnable.
- **Failed attempt:** scoring `data/examples/role-scores.json` (a scorer output, not valid input) exited 0 with all composites 0 and no warning. Reproduced 2026-10-20; not fixed.
- **Open issues:** `role_quality` weight is 0 (inert); missing `liveness`/`timeline` defaults to open gate; `consider_floor` unpinned; all values synthetic, so no live-posting or OPT-eligibility check; input guard not built; 6 typed TODOs open in the recipe.
- **Gates cleared by:** none. No human attestation recorded.
