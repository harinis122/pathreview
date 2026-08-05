## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/149

**Issue title:** Structural chunker silently drops documents that contain no headings

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3
I picked a tier 1 issue because I have never made an open source contribution before and wanted to start with something easy. This issue seems relatively easy while still needing to be fixed for the program to function properly.

**Problem summary:**
This issue is about the system not considering documents that do not contain any headers. This issue is present because the structural chunking algorithm (in chunking/structural_chunker.py, specifically in the chunk function in StructuralChunker class) extracts chunks of the doc by chunking at present headers. With no headers present, the function _extract_sections does not extract any sections and basically views the doc as empty, which is the bug. A successful fix would ensure that documents without headers do not get dropped, but rather for these kinds of documents, an alternate chunking technique is used.

**Branch name:** fix/149-chunker-drops-docs-without-heading

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** 
https://github.com/harinis122/pathreview/commit/a0fa237fad74f14f838d522302bb80a37b0b38e6

**Reproduction summary:**
I reproduced the issue by going through the structural_chunker test cases (in test_structural_chunker.py) and finding a relevant test case which tests functionality of program with headingless documents. Since the test case was already there and failed because the program currently ignores headerless documents, I added a comment above that test case to indicate this is the reproduced issue.

**PLAN.md link:** 
https://github.com/harinis122/pathreview/blob/fix/149-chunker-drops-docs-without-heading/PLAN.md

**Blockers or open questions:**
How can we properly test the functionality of the program and ensure our issue is properly fixed if the program cannot be run due to multiple other issues present?



## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
So far, I have attempted to fix the bug in the project (Structural chunker silently drops documents that contain no headings). I removed the condition in _extract_sections() in structural_chunker.py that only starts saving text once there has been at least one heading. I indirectly added a fallback chunking path for non-empty documents without headers as in PLAN.md and got through step 3.

**Next steps:**
For the rest of the week, I will work on testing my fixed code, making sure it passes for all edge cases.

**Blockers:**
N/A

---

### Check-in 2 (end of week)

**PR link:** https://github.com/harinis122/pathreview/pull/1

**Branch:** fix/149-chunker-drops-docs-without-heading

**What you built:**
I fixed `_extract_sections()` in `ingestion/chunking/structural_chunker.py` so it buffers and saves content lines regardless of whether a heading has been seen yet, using an empty heading path (`""`) and level `0` for text with no heading context. This means headerless documents (and any leading text before a document's first heading) are now preserved as chunks instead of being silently dropped, and they automatically flow through the existing size-based logic in `chunk()` — short headerless docs become a single chunk, long ones get sub-chunked via `SemanticChunker`, just like oversized headed sections already did.

**Tests added or updated:**
I updated `tests/unit/test_structural_chunker.py`: removed the stale "this test fails" comment above `test_document_with_no_headings` now that it passes, and added `test_short_headerless_document_single_chunk`, `test_long_headerless_document_sub_chunked`, and `test_content_before_first_heading_preserved` to cover a short headerless doc, a long headerless doc that triggers semantic sub-chunking, and text appearing before a document's first heading.

**Self-review confirmation:** [X] make check passes  [X] make test-unit passes

**Draft PR feedback received from:** none



## Week 10 — Iteration & reflection

### Reviewer feedback

**Feedback received:** [ ] Yes  [X] No — still awaiting review

**Summary of feedback:**
N/A: no reviews

**How you responded:**
N/A: no reviews


---

### Reflection

**What was harder than you expected?**
[Be specific — what part of the process, codebase, or workflow
surprised you?]
Understanding the codebase and figuring out what the bug was was the hardest part for me. I found it a bit difficult to navigate the codebase, figure out where each logic/test case was, and run the program. This surprised me, as writing code was definitely the easier part.

**What did you learn about working in a large codebase?**
I learned that working in a large codebase is much different from building my own project, but one is not necessarily harder or easier than the other. For large codebases, I think the most difficult part is understanding the codebase to ensure you only change the part that needs change, and to avoid unexpected side effects. However, when building your own project, the tough part is making sure the system design is solid, and understanding what problem the project should solve.

**How did AI tools help — and where did they fall short?**
AI tools were very helpful when trying to understand the codebase as a whole because it summarizes the function of each part/each file really well. I relied on AI tools to help me quickly understand what was going on in the code base and to figure out where my issue was occuring in the codebase. Without AI it would have taken me significantly longer to figure this out. However, AI tools generally fell short when exactly pinpointing the issue and understanding exactly what to fix. Although I did use AI tools to fix my issue, I did have to prompt the AI well and explicitly specify what changes it should make (nothing more), and I do not think the fix would have been successful if I did not prompt it well.

**What would you do differently if you started over?**
I think my project went pretty well and I don't think there is anything in particular I would do differently. However, if I were to restart, I would have picked a harder issue to work on since the issue I picked was a bit too easy.

**What are you most proud of from this module?**
I am proud that I learned how to use AI efficiently to understand the codebase but did not rely on it to do the work entirely for me. I used AI to understand the codebase and figure out the general area of my issue and then pinpointed the issue myself, and prompted AI to fix the issue. I like that I did the important thinking myself and used AI for more repetitive, manual tasks!
