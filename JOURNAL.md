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

**PR link:** [link to your submitted pull request]

**Branch:** [the branch name you worked on, e.g. `fix/123-short-description`]

**What you built:**
[1–3 sentences summarizing what your fix does and how it works]

**Tests added or updated:**
[Which test files did you touch? What do they cover?]

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

**Draft PR feedback received from:** [name or Slack handle, or "none"]