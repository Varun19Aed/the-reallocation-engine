# Domain Justification — OPT-Aware Frontend Job Opportunity Evaluator

## Executive summary

**What this is.** A one-page argument for why an international student looking
for frontend engineering work needs a scoring procedure that treats "is the
posting still open?" and "does the timing fit my work permit?" as hard stops.

**Why read it.** It names the specific information gap the procedure closes and
two ways it could mislead a student without being noticed.

**What it concludes.** The procedure is worth having because the two things
that most often waste a student's limited time (a dead posting, an impossible
start date) are invisible from a job listing. It is only as good as the inputs
people give it, and so far those inputs are invented sample values.

## Who and when

An international master's student in a frontend-relevant field on F-1 OPT, or
STEM OPT, with a fixed authorization end date, choosing which 3–30 frontend or
React roles deserve a tailored application this week. The recipe is
`recipes/case-frontend-opt-sponsorship.md`.

## The information asymmetry

From a job listing the student cannot easily see:
1. whether the employer has a record of sponsoring visas (the listing rarely says);
2. whether the posting is still being filled, or is a stale or "ghost" listing;
3. whether the hiring timeline fits their authorization clock.

Employers and recruiters know these; the candidate usually does not. Application
effort is scarce, so each wasted application is a real cost against a fixed
deadline.

## Engine layers used

- **80 Days to Stay:** sponsorship history is the source for the sponsorship probability. *In the sample run this was a typed-in synthetic number, not read from the dataset.*
- **Job-Ops:** posting liveness is a gate. *In the sample run it was a typed-in synthetic factor; `npm run ats:liveness` was not run.*
- **Cognitive Pivot:** role quality is wired in but weighted 0, so it contributes nothing yet.

## Failure modes

**1. Optimistic default on a missing gate.** If a record omits `liveness` or
`timeline`, the scorer treats it as 1 (open) without a warning. A role the
student never checked can therefore score Apply. The output looks identical to a
checked role. Hardest to catch for a student who is new to the system and reads
a clean Apply as "verified"; the audit line shows the `1` but does not say it was
assumed.

**2. Precise-looking sponsorship probability with no real basis.** A value such
as 0.9 reads as a measurement but is a judgment about an employer, and an
employer's past sponsorship does not guarantee it will sponsor this role or this
person. Hardest to catch for a student under deadline pressure, who has no way to
audit the number and every reason to believe it. Related: the sample's `fit`
score is a model judgment, labeled as such in the trace, but it moves the
composite as much as sponsorship does.

## Limits

Nothing here is immigration or legal advice, and the scorer encodes no OPT rules.
All sample inputs are fictional. The role-quality weight is an unresolved
authorial decision.
