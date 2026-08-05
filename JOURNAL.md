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

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I've implemented the fix to the code for the specified bug in the system (steps 1 and 2 of my plan). To do this, I added filtering checks for source_type and routed the source_id check to match with the content_hash in the IngestedSource model. 

**Next steps:**
I'm working on creating a test suite to unit tests the individual pipeline functions and also integration tests convering deduplication for every type of IngestedSource (resume, repo, etc.)

**Blockers:**
I'm struggling to narrow the scope of my issue, seeing as there are a lot of surrounding bugs that surface when probing for this specific issue.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/965

**Branch:** fix/6-duplicate-embeddings

**What you built:**
Fixed duplicate embedding generation by implementing proper deduplication in the ingestion pipeline. The solution queries the IngestedSource database table using three key fields (content_hash, source_type, and profile_id) to check if a source has already been ingested. When a duplicate is detected, the ingestion is skipped, preventing duplicate embeddings from being stored in the vector database. The fix ensures that each unique source (by content, type, and profile) is only ingested once, eliminating inflated retrieval scores from identical chunks appearing multiple times.

**Tests added or updated:**
- Created `tests/unit/test_ingestion_pipeline.py` with 16 unit tests covering `_check_skip()`, `ingest_resume()`, `ingest_repo_metadata()`, `ingest_readme()`, `_record_ingested_source()`, and utility functions
- Created `tests/integration/test_ingestion_deduplication.py` with 6 integration tests covering end-to-end deduplication scenarios, edge cases (different profiles, different source types), and concurrent ingestions
- All 22 tests pass pytest collection and validation

**Self-review confirmation:** [X] make check passes  [X] make test-unit passes

**Draft PR feedback received from:** None