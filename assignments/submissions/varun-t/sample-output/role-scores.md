# Role Scorer report — 2026-10-20

*Bayesian Role Scorer (Ch.11). Weights: sponsorship 0.35, fit 0.3, role_quality 0 [role_quality weight is **[VERIFY]** — not pinned by the chapter]. Threshold 0.3. Profile requires sponsorship.*

**Summary:** 4 roles → Apply 1 · Consider 1 · Skip 2. **Skip rate 50%** (healthy — a good run skips at least half).

| Role | Composite | Rec | Why | Audit (term · value · weight · source) |
|---|---|---|---|---|
| Fictional Company A — Frontend Engineer | 0.570 | **Apply** | composite 0.570 ≥ 0.3, gates healthy | sponsorship 0.9·0.35 [synthetic-fixture]; fit 0.85·0.3 [synthetic-model-judgment]; role_quality 0.8·0 [synthetic-fixture] × liveness 1[synthetic-fixture]×timeline 1[synthetic-fixture] |
| Fictional Company D — Frontend Engineer | 0.418 | **Consider** | above threshold (0.418) but one soft spot: sponsorship tier "likely" | sponsorship 0.6·0.35 [synthetic-fixture]; fit 0.85·0.3 [synthetic-model-judgment] × liveness 1[synthetic-fixture]×timeline 0.9[synthetic-fixture] |
| Fictional Company B — Full Stack Engineer | 0.000 | **Skip** | gated: liveness ≈ 0.000 (a closed gate zeroes the composite regardless of votes) | sponsorship 0.95·0.35 [synthetic-fixture]; fit 0.9·0.3 [synthetic-model-judgment] × liveness 0[synthetic-fixture]×timeline 1[synthetic-fixture] |
| Fictional Company C — React Developer | 0.000 | **Skip** | gated: timeline ≈ 0.000 (a closed gate zeroes the composite regardless of votes) | sponsorship 0.9·0.35 [synthetic-fixture]; fit 0.9·0.3 [synthetic-model-judgment] × liveness 1[synthetic-fixture]×timeline 0[synthetic-fixture] |

*Every term traces to its source. If you cannot explain a row term-by-term, distrust the recommendation before your confusion (Ch.11).*
