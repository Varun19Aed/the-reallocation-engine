# Reallocation Engine — Recipe Design Assignment

## Student

Varun Tadimeti

## GitHub handle

`varun-Tad`

## Contribution

**Frontend / Full-Stack Sponsorship Triage**

## Lifecycle status

`RUNNABLE-SAMPLE`

## Branch

`contrib/2026fa-varun-frontend-triage`

## Recipe

`recipes/cases/2026fa/varun-Tad-frontend-triage.md`

## Human card

`recipes/cases/2026fa/varun-Tad-frontend-triage.card.md`

## Prototype

`scripts/contrib/2026fa/varun-Tad-frontend-triage/`

## Run log

`logs/runs/2026fa-varun-Tad-1.md`

## Supporting documentation

- `course/2026fa/submissions/varun-Tad/CHANGE-BRIEF.md`
- `course/2026fa/submissions/varun-Tad/DOMAIN-JUSTIFICATION.md`
- `course/2026fa/submissions/varun-Tad/WORKED-RUN.md`
- `course/2026fa/submissions/varun-Tad/TEST-REPORT.md`
- `course/2026fa/submissions/varun-Tad/SOURCES.md`

## Validation summary

- `npm run verify` — PASS
- `npm run doctor` — PASS / environment runnable
- Offline prototype tests — PASS, 4/4
- ATS dry-run — PASS after repository-documented configuration setup
- Job-posting liveness sample — PASS, tested URL reported active
- Existing role scorer sample — PASS
- PII scan — one finding in the repository's existing `package-lock.json`; no contribution file was identified by the scan

## Evidence boundary

The prototype reports historical sponsorship evidence available in the repository. It does not claim that historical sponsorship establishes sponsorship for a current role.

Missing sponsorship evidence remains unknown rather than being interpreted as evidence of non-sponsorship.

The contribution does not convert historical H-1B approval rate directly into a sponsorship probability because the locally available evidence is insufficient to reproduce and validate the repository's complete documented sponsorship model.

The final decision to apply, research further, contact the employer, or skip remains with the human user.

## Attestation

### Tested

- Historical sponsorship evidence retrieval
- Company legal-suffix normalization
- Missing sponsorship evidence behavior
- Company-not-found behavior
- Required-argument failure behavior
- Offline fixture tests
- Existing scorer sample
- ATS dry-run
- Real job-posting liveness
- Repository conformance and doctor checks

### Did not test

- General fuzzy company entity resolution
- Current sponsorship availability for the sample companies or roles
- Complete raw DOL/LCA entity-resolution accuracy
- Full reconstruction of the repository sponsorship probability
- Batch behavior across every company record
- Future liveness of the tested job posting

### Broke during testing and fixed

The first company matcher failed to resolve `A10 Networks` to the stored legal name `A10 NETWORKS INC`. Deterministic legal-suffix normalization was added and verified with an offline test.

An early implementation also placed evidence extraction outside the intended execution scope and produced a `NameError`. The code was moved into the correct scope and the tests were rerun successfully.

The ATS dry-run initially failed because `portals.yml` had not been configured. The repository's documented example configuration was used.

The liveness check initially could not launch Chromium. The required Playwright Chromium dependency was installed and the check was rerun successfully.

## Commit

To be filled with the final Git commit SHA after the submission commit is created.

## Pull request

To be filled with the final GitHub pull-request URL after the branch is pushed.
