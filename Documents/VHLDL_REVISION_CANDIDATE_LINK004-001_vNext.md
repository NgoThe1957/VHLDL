# VHLDL Revision Candidate — LINK004-001 vNext

> **Status: CANDIDATE — NOT FROZEN**
>
> This document defines the controlled revision candidate for the current Golden/Regression cycle. It does not replace the current baseline until the required regression evidence is reviewed and accepted.

## 1. Purpose

This candidate addresses the controlled revision of Markdown extraction, page traceability, chapter-heading classification, and cross-reference evidence for the VHLDL pilot artifacts.

## 2. Locked Change Requirements

### MCR-LINK004-001 — Chapter Heading vs Running Header
Distinguish true chapter headings from running headers/page headers using page position, structure, and contextual evidence. A repeated running header must not be promoted to a chapter heading.

### MCR-LINK004-002 — Header/Footer/Page Number Classification
Classify header, footer, and page-number material from PDF evidence. Do not delete numeric content merely because it resembles a page number when it belongs to the source content.

### MCR-LINK004-003 — Chapter Heading OCR Normalization
Normalize OCR errors in chapter headings without changing source content, order, meaning, or heading hierarchy.

### MCR-LINK004-004 — Deterministic PDF Page Anchors
For every represented PDF page, generate exactly one deterministic anchor in the form `#pdf-page-N`, where `N` is the technical PDF page number. Anchors must be unique, exact, and resistant to prefix collisions (`#pdf-page-36` must not be confused with `#pdf-page-360`).

### MCR-LINK004-005 — PDF Page ↔ Markdown Evidence
A page-mapping claim is PASS only when the PDF page and the corresponding Markdown page marker/anchor can be independently evidenced. No page mapping may be extrapolated from a single sample without a validated rule.

### MCR-LINK004-006 — Printed Page Separation
`Printed_Page` is a separate field from `PDF_Page`. It may be populated only when the printed page number is independently evidenced from the source. Otherwise it remains `UNKNOWN`.

### MCR-LINK004-007 — Actual Cross-file Markdown Links
A cross-reference is PASS only when an actual Markdown link exists, resolves to the intended target artifact, and points to the intended anchor. A Workbook record alone is insufficient evidence.

### MCR-LINK004-008 — Cross-link Target Integrity
For each accepted cross-file link, verify source file, target file, target anchor, and link syntax. Broken, missing, or ambiguous links remain HOLD.

## 3. Regression Requirement

The LSVN_001 PDF evidence currently establishes at least:
- PDF Page 144 ↔ Printed Page 142
- PDF Page 145 ↔ Printed Page 143

These are regression evidence points. They must not be converted into a universal page-offset assumption without broader validation.

## 4. Acceptance Gates

- Anchor Gate: all required pilot anchors exist exactly once.
- Page Mapping Gate: PDF Page ↔ Printed Page ↔ Markdown is evidenced where claimed.
- Link Gate: actual cross-file Markdown links are present and resolve correctly.
- Entity Gate: entity ID/type/source evidence remain valid and non-duplicated.
- Golden Acceptance remains HOLD until the complete required regression evidence passes.

## 5. Baseline Protection

The current five-sheet `Index.xlsx` remains the baseline. The candidate 14-sheet data model does not replace it without a separate approved Change Request. Source PDFs are immutable and are never modified to satisfy QA.
