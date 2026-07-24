# Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/153

**Issue title:** Faithfulness checker crashes when a context chunk has text: None

**Tier:** [x] Tier 1

**Why this issue:** I picked a Tier 1 issue for my first pass through this codebase since I'm still building familiarity with the RAG evaluator module and wanted a contained bug I could reason about end-to-end rather than a multi-file feature. The scope was a good fit: the crash is isolated to one method (`FaithfulnessChecker.check()`), the root cause is a single line of faulty `None`-handling, and there's already a failing test (`test_none_context_chunk_text`) that pins down the expected behavior — so I could verify my fix without having to design new test coverage myself.

**Problem summary:**

`FaithfulnessChecker.check()` in [rag/evaluator/faithfulness_checker.py](rag/evaluator/faithfulness_checker.py) builds the combined context string with `chunk.get("text", "")` before joining all chunks together, but `dict.get`'s default only kicks in when the key is *missing* — if a chunk explicitly has `"text": None`, `.get` still returns `None`. That `None` then lands in the list passed to `" ".join(...)`, which raises a `TypeError` because `join` requires every item to be a string. In practice this means any retrieved context chunk with a null `text` field (e.g. a chunk that failed to embed content, or a placeholder record) crashes the whole faithfulness check instead of just being treated as empty. A successful fix should coerce a `None` text value to an empty string (or otherwise skip that chunk) so `check()` returns a normal float score instead of throwing, which is exactly what `test_none_context_chunk_text` in [tests/unit/test_faithfulness_checker.py](tests/unit/test_faithfulness_checker.py) asserts.

**Branch name:** fix/153-faithfulness-checker-crashes

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger
