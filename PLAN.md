## Solution plan

**Issue:** Structural chunker silently drops documents that contain no headings (https://github.com/ascherj/pathreview/issues/149)

### Understand

**Root cause:**
`StructuralChunker.chunk()` relies on `_extract_sections()` to split a document based on headers. When a document contains no headers, `_extract_sections()` returns no sections. The chunker then treats the document as empty and returns no chunks, even though the document contains valid text.

**Expected behavior:**
A document without headers should still be chunked using a fallback strategy, such as semantic or size-based chunking.

**Actual behavior:**
The document is dropped and produces an empty chunk list.

### Map

Likely files and components involved:

* `ingestion/chunking/structural_chunker.py`

  * `StructuralChunker.chunk()`
  * `StructuralChunker._extract_sections()`
* `ingestion/chunking/semantic_chunker.py`

  * Potential fallback chunking implementation
* `tests/unit/test_structural_chunker.py`

  * Existing or updated regression tests for headerless documents

Expected files to touch:

* `ingestion/chunking/structural_chunker.py`
* `tests/unit/test_structural_chunker.py`

Possibly:

* `ingestion/chunking/semantic_chunker.py` (if changes are needed to support reuse as a fallback)

### Plan

1. Reproduce the issue by running the existing headerless-document test or confirming that `StructuralChunker.chunk()` returns no chunks.
2. Update `StructuralChunker.chunk()` to detect when `_extract_sections()` returns no sections.
3. Add a fallback chunking path for non-empty documents without headers (using semantic_chunker).
4. Add regression tests asserting that headerless content is preserved for documents of various lengths.
5. Run the focused tests and the repository’s formatting, linting, and type-checking tools.

### Inputs & outputs

**Input:**
A document containing text, with or without structural headers.

Example:

```text
This document contains useful content but has no headings.
```

**Expected output:**
One or more chunk objects containing the original document content.

The fix should change the behavior from:

```python
[]
```

to:

```python
[
    Chunk(text="This document contains useful content but has no headings.", metadata= {"heading_path": heading_path,
                    "heading_level": 0,
                    "chunk_index": 0,
                    "char_start": 0,
                    "char_end": 56})
]
```


### Risks & unknowns

* It is unclear which fallback chunking algorithm would be optimal.
* Reusing `SemanticChunker` could introduce dependencies, configuration requirements, or circular imports.
* Returning the entire document as one chunk may violate existing maximum chunk-size limits.
* Metadata normally derived from headers may be unavailable for headerless documents.
* Whitespace-only documents must not accidentally produce meaningless chunks.
* The fallback behavior should not alter documents that already contain valid headers.

### Edge cases

The fix should handle:

* A normal document with multiple headers
* A document with no headers but valid text
* A short single-line document (headerless)
* A long headerless document requiring multiple chunks
* An empty document
* A whitespace-only document (treated like empty document)
* A document containing text before its first header
* Malformed or inconsistent header syntax
* Documents containing only a title or header with no body
* Unicode, punctuation, and multiline text without headers
