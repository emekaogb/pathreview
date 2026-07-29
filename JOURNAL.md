## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/6

**Issue title:** Duplicate embeddings generated when re-ingesting the same repository

**Tier:** [ ] Tier 1  [X] Tier 2  [ ] Tier 3

**Problem summary:**
The pipeline.py doesn't check whether a repo has already been ingested before processing it. Re-uploading the same GitHub repo generates duplicate vector entries, causing retrieval to return identical chunks with inflated scores. The relevant files are ingestion/pipeline.py and core/models/ingested_source.py.

**Branch name:** fix/6-duplicate-embeddings

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
I ingested a GitHub repo for the first time and then re-uploaded the same repo with identical data. The result would be a second ingestion that isn't skipped. New chunks would be created and embeddings generated again, resulting in the vector database having duplicate entries of the same content.
- Logged error: github_ingestion_failed  error="'raw_data' is an invalid keyword argument for IngestedSource" request_id=2ca79f09-8bce-4bba-a85d-8b811fdecfaa username=emekaogb

**PLAN.md link:** ./PLAN.md 

**Blockers or open questions:**
I'm uncertain about the scope of the bug I've been assigned. There are a lot of connected parts and I want to make sure I'm applying fixes to only the aspects that are related to my issue. Also, the database model being used for this repo has many placeholders and doesn't seem to be active, so it makes testing harder.