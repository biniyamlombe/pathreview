# PathReview Contribution Journal

## Week 7 - Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/158

**Issue title:** review_service unit tests misconfigure async mocks — 13 of 19 tests fail

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The unit tests for `ReviewService` mock the database session incorrectly for async SQLAlchemy. The service code correctly uses `await db.execute(...)`, but the tests make `result.scalars()` return a coroutine instead of a normal result object. That causes `AttributeError: 'coroutine' object has no attribute 'first'` (and `'all'`) in 13 of 19 tests. A successful fix will rework the mocks so `execute` stays an `AsyncMock` while the returned result uses a sync `MagicMock`/`Mock` for `.scalars().first()` / `.all()`, so the existing CRUD tests pass. This affects `tests/unit/test_review_service.py` and how it mocks calls used by `core/services/review_service.py`.

**Branch name:** `fix/158-review-service-async-mocks`

**Setup confirmation:** [ ] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
