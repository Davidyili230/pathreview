# Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/153

**Issue title:** Faithfulness checker crashes when a context chunk has text: None

**Tier:** [x] Tier 1

**Problem summary:**

`FaithfulnessChecker.check()` in [rag/evaluator/faithfulness_checker.py](rag/evaluator/faithfulness_checker.py) builds the combined context string with `chunk.get("text", "")` before joining all chunks together, but `dict.get`'s default only kicks in when the key is *missing* — if a chunk explicitly has `"text": None`, `.get` still returns `None`. That `None` then lands in the list passed to `" ".join(...)`, which raises a `TypeError` because `join` requires every item to be a string. In practice this means any retrieved context chunk with a null `text` field (e.g. a chunk that failed to embed content, or a placeholder record) crashes the whole faithfulness check instead of just being treated as empty. A successful fix should coerce a `None` text value to an empty string (or otherwise skip that chunk) so `check()` returns a normal float score instead of throwing, which is exactly what `test_none_context_chunk_text` in [tests/unit/test_faithfulness_checker.py](tests/unit/test_faithfulness_checker.py) asserts.

**Branch name:** fix/153-faithfulness-checker-crashes

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger
