## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/6

**Issue title:** Duplicate embeddings generated when re-ingesting the same repository

**Tier:** [ ] Tier 1  [X] Tier 2  [ ] Tier 3

**Problem summary:**
The pipeline.py doesn't check whether a repo has already been ingested before processing it. Re-uploading the same GitHub repo generates duplicate vector entries, causing retrieval to return identical chunks with inflated scores. The relevant files are ingestion/pipeline.py and core/models/ingested_source.py.

**Branch name:** [paste branch name here]

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger