# GPT-5.2 rubric supplement

`paper_artifact_rubric_supplement.csv` releases the **GPT-5.2 rubric quality scores**
that back the paper's claim that, on the well-formed Cornelius dataset, retrieval (M3)
and repair (M4) score *lower overall quality* than the constrained zero-shot rewrite (M2)
— the "M2 0.732 vs. M4 0.673" divergence (Sec. V, construct-validity).

## Why a separate file
The main artifact (`paper_artifact.csv`) releases the deterministic **RQI2** index and the
**binary** GPT-5.2 validity per item. The *rubric overall-quality* scores were computed in a
separate audit step and were not part of the original CSV; this file adds them.

## Provenance
- Source: `alternate.ipynb`, audit cells (rubric definition + aggregation; Spearman cell).
- Sample: `df_audit = df_clean.sample(AUDIT_N, random_state=42)` with **AUDIT_N = 500**
  (a 500-item subsample of the cleaned Cornelius set; the rubric audit is intentionally a
  subsample because each item requires a GPT-5.2 API call).
- Judge: GPT-5.2, rubric-based, **1–5 per dimension** (atomicity, testability, unambiguity,
  conciseness, faithfulness); `overall_0to1 = (sum of the five dimensions) / 25`.
- Values in this file are the executed-run aggregates (means over the 500 audited items).
  The paper rounds these (e.g. M2 0.7307 → 0.732; M3 0.6679 / M4 0.6702 → ~0.673).

## What this shows
- Overall quality: **M2 0.731 > M4 0.670 > M3 0.668** — retrieval lowers rubric quality on
  clean inputs; the repair loop does not recover it (consistent with M4 being near-inert here).
- The drop is concentrated in **faithfulness** (M2 3.24 → M3 2.62) and **conciseness**
  (4.83 → 4.53), i.e. retrieval adds unfaithful / wordier material — the leakage failure mode.
- RQI2 vs rubric-overall Spearman: M2 0.372, M3 0.368, M4 0.384 (moderate; RQI2 captures
  structure, not intent).

## Regenerating per-item scores
Per-item rubric scores are not stored on disk (they were held in the in-memory `df_audit`).
To regenerate them, set `OPENAI_API_KEY`, run the rubric audit cells in `alternate.ipynb`
with the same `random_state=42`, and dump `df_audit[[c for c in df_audit.columns if '_llm_' in c]]`.
