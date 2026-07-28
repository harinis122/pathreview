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

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
[1–2 sentences: How did you reproduce the issue? What did you observe?]

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]